It is possible to debug Betaflight configurator with Visual Studio Code which is a more friendly environment the DevTools. 
This procedure has been tested only for Windows 10. 

Run Betaflight in debug mode (launch: yarn gulp debug)

In VSCode, create a launch.json file with this configuration:
<br/><br/>
`
{  
    "version": "0.2.0",
    "configurations": [  
          {
            "name": "Launch Betaflight",
            "type": "nwjs",
            "request": "launch",
            "runtimeExecutable": "TO BE REPLACED BY THE PATH TO BETA FLIGHT DEBUG MODE",
            "runtimeArgs": [
                "${workspaceRoot}",
                "--remote-debugging-port=9222"
            ],
            "port": 9222,
            "nwjsVersion": "any",
            "webRoot":"TO BE REPLACED BY THE PATH TO BETA FLIGHT DEBUG MODE"
        },
        {
            "type": "nwjs",
            "request": "attach",
            "name": "Attach to Betaflight",
            "port": 9222,
            "webRoot":"TO BE REPLACED BY THE PATH TO BETA FLIGHT DEBUG MODE/js",
            "verbose":true,
            "reloadAfterAttached": true
        }            
    ]
}
`
Then you should be able to debug betaflight configurator.


