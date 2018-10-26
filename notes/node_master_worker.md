# Master worker snippet for Node.js

```js
const runMasterWorker = () => {
	const COMMANDS = {
		GET_WORK: 'GET_WORK',
		WORK: 'WORK',
		SHUTDOWN: 'SHUTDOWN'
	};

	if (cluster.isMaster) {
		let workQueue = [1, 2, 3, 4, 5, 6, 7, 8, 9];

		cluster.on('online', worker => {
			console.info(`[MASTER] Worker ${worker.id} is online!`);
		});

		cluster.on('exit', (worker, code, signal) => {
			if (code === 0) {
				console.info(`[MASTER] Worker ${worker.id} exited with code '${code}'.`);
			} else {
				console.error(
					`[MASTER] Worker ${worker.id} exited with code '${code}' and signal '${signal}'.`
				);
			}
		});

		cluster.on('message', (worker, msg) => {
			switch (msg.command) {
				case COMMANDS.GET_WORK:
					const work = workQueue.pop();
					if (work) {
						console.info(`[MASTER] Sending work to worker ${worker.id}...`);
						worker.send({
							command: COMMANDS.WORK,
							data: work
						});
					} else {
						worker.send({
							command: COMMANDS.SHUTDOWN
						});
					}
					break;
			}
		});

		const numCPUs = require('os').cpus().length;
		for (let i = 0; i < numCPUs; i++) {
			cluster.fork();
		}
	} else {
		process.send({
			command: COMMANDS.GET_WORK
		});

		process.on('message', msg => {
			switch (msg.command) {
				case COMMANDS.SHUTDOWN:
					console.info(`[WORKER ${cluster.worker.id}] Master has tell me to shutdown.`);
					process.exit(0);
				case COMMANDS.WORK:
					console.info(`[WORKER ${cluster.worker.id}] Received work from master.`);
					doWork(msg.data).then(() =>
						process.send({
							command: COMMANDS.GET_WORK
						})
					).catch(err => {
						console.error(`[WORKER ${cluster.worker.id}] Error doing work:`, err);
						process.exit(1);
					});
					break;
			}
		});

		const doWork = async work => {
			if (work === 2) {
				throw 'I do not like number 2.';
			}
			console.info(`Doing work reveiced from master... ${work}`);
		};
	}
};
```
