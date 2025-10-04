## LAB 3 - Extra instructions

### Launching virtual Furhat and Start Remote API:

When trying to launch the SDK for the first time, I got some errors that I solved by making the AppImage file (furhat-sdk-desktop-launcher.AppImage) executable and running it directly from the terminal. That is: 
```
chmod +x furhat-sdk-desktop-launcher.AppImage
```
```
./furhat-sdk-desktop-launcher.AppImage
```
After that, Furhat Studio opens and we can now click on <mark>Lunch Virtual Furhat</mark>.
Next, we need to start the remote API by either clicking on <mark>Open web interface</mark> or <mark>Remote API</mark>. This will open a window in the browser where you can see the control panel with settings, gestures, etc.


### Furhat & XState starter:

After we've forked and cloned the <a href=https://github.com/vladmaraev/xstate-furhat-starter> xstate-furhat-starter</a> repo, now it's time to create and add the gestures for Furhat. There's a section dedicated to gestures on the Furhat documentation: https://docs.furhat.io/gestures/

To run the code, just follow the commands in the repo, that is:
```
yarn tsx src/main.ts
```
To run in watch mode:
```
yarn tsx watch src/main.ts
```

If you get an error like this one below, it might have something to do with the node.js version you're currently using:

```
Start
Next
{
  type: 'xstate.error.actor.0.root.Next',
  error: /home/xstate-furhat-starter/src/main.ts:17
    const myHeaders = new Headers();
                      ^
  
  ReferenceError: Headers is not defined
      at fhSay (/home/xstate-furhat-starter/src/main.ts:17:21)
      at <anonymous> (/home/xstate-furhat-starter/src/main.ts:89:14)
      at Object.start (/home/xstate-furhat-starter/node_modules/xstate/actors/dist/xstate-actors.cjs.js:791:47)
      at Actor.start (/home/xstate-furhat-starter/node_modules/xstate/dist/raise-b4a670a0.cjs.js:841:20)
      at /home/xstate-furhat-starter/node_modules/xstate/dist/raise-b4a670a0.cjs.js:1210:14
      at Actor.update (/home/xstate-furhat-starter/node_modules/xstate/dist/raise-b4a670a0.cjs.js:623:9)
      at Actor._process (/home/xstate-furhat-starter/node_modules/xstate/dist/raise-b4a670a0.cjs.js:886:10)
      at Mailbox.flush (/home/xstate-furhat-starter/node_modules/xstate/dist/raise-b4a670a0.cjs.js:45:12)
      at Mailbox.enqueue (/home/xstate-furhat-starter/node_modules/xstate/dist/raise-b4a670a0.cjs.js:37:12)
      at Actor._send (/home/xstate-furhat-starter/node_modules/xstate/dist/raise-b4a670a0.cjs.js:984:18),
  actorId: '0.root.Next'
}
Fail
```

I solved this issue by installing version 22 and setting it as default (so I don't run into the same error every time I run the code):
```
nvm install 22
```
```
nvm alias default 22
```

Otherwise, if the program runs successfully, you should see something like this (but the output might be different if you have modify the code in main.ts):
```
Start
Next
Response {
  status: 200,
  statusText: 'OK',
  headers: Headers {
    date: 'Fri, 03 Oct 2025 13:25:18 GMT',
    server: 'Application/debug ktor/debug',
    'content-length': '51',
    'content-type': 'application/json; charset=UTF-8',
    connection: 'keep-alive'
  },
  body: ReadableStream { locked: false, state: 'readable', supportsBYOB: true },
  bodyUsed: false,
  ok: true,
  redirected: false,
  type: 'basic',
  url: 'http://127.0.0.1:54321/furhat/say?text=Hi&blocking=true'
}
Listen
```

Remember that you need to launch virtual Furhat and start the remote API before running main.ts, otherwise you'll get:
> Error: connect ECONNREFUSED 127.0.0.1:54321.
