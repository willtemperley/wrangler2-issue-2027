# Reproducing https://github.com/cloudflare/wrangler2/issues/2027

`npm run dev` executes the `turbo run dev --parallel` script.

This starts multiple wrangler processes in parallel (workers w0, w1 and w2 in ./packages)

In the case shown below, w2 fails to start. Note: nothing was listening on port 6284 prior to running this.

It may take a few tries to replicate the issue. 

Running separate wrangler processes in parallel causes the 

```shell
w2:dev: > w2@0.0.0 dev
w2:dev: > wrangler dev
w2:dev: 
w1:dev: 
w1:dev: > w1@0.0.0 dev
w1:dev: > wrangler dev
w1:dev: 
w2:dev:  ⛅️ wrangler 2.1.11 
w2:dev: --------------------
w0:dev:  ⛅️ wrangler 2.1.11 
w0:dev: --------------------
w2:dev: ⬣ Listening at http://0.0.0.0:8789
- http://127.0.0.1:8789
- http://192.168.1.90:8789
w2:dev: 
w0:dev: /Users/will/cloudflare/wrangler-issue-2027/node_modules/wrangler/wrangler-dist/cli.js:25672
w0:dev:             throw ex;
w0:dev:             ^
w0:dev: 
w0:dev: Error: listen EADDRINUSE: address already in use :::6284
w0:dev:     at Server.setupListenHandle [as _listen2] (node:net:1330:16)
w0:dev:     at listenInCluster (node:net:1378:12)
w0:dev:     at Server.listen (node:net:1465:7)
w0:dev:     at startWorkerRegistry (/Users/will/cloudflare/wrangler-issue-2027/node_modules/wrangler/wrangler-dist/cli.js:138993:12)
w0:dev:     at processTicksAndRejections (node:internal/process/task_queues:96:5)
w0:dev: Emitted 'error' event on Server instance at:
w0:dev:     at emitErrorNT (node:net:1357:8)
w0:dev:     at processTicksAndRejections (node:internal/process/task_queues:83:21) {
w0:dev:   code: 'EADDRINUSE',
w0:dev:   errno: -48,
w0:dev:   syscall: 'listen',
w0:dev:   address: '::',
w0:dev:   port: 6284
w0:dev: }
w0:dev: npm ERR! Lifecycle script `dev` failed with error: 
w0:dev: npm ERR! Error: command failed 
w0:dev: npm ERR!   in workspace: w0@0.0.0 
w0:dev: npm ERR!   at location: /Users/will/cloudflare/wrangler-issue-2027/packages/w0 
w0:dev: ERROR: command finished with error: command (/Users/will/cloudflare/wrangler-issue-2027/packages/w0) npm run dev exited (1)
w2:dev: ✘ [ERROR] Watch build failed: Error: The service was stopped
w2:dev: 
w2:dev:       at Socket.afterClose (/Users/will/cloudflare/wrangler-issue-2027/node_modules/esbuild/lib/main.js:662:18)
w2:dev:       at Socket.emit (node:events:538:35)
w2:dev:       at endReadableNT (node:internal/streams/readable:1345:12)
w2:dev:       at processTicksAndRejections (node:internal/process/task_queues:83:21)
w2:dev: 
```