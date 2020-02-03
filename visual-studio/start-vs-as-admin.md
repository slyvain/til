# Start Visual Studio As Administrator

_This has been personally tested and confirmed with Windows 10 for Visual Studio 2017 and 2019._

To *always* start Visual Studio as administrator and prevent such message:
![Elevated Permissions Required In Visual Studio](../assets/vs-elevated-permissions.png?raw=true)

1. Navigate to the location of _devenv.exe_ (`C:\Program Files (x86)\Microsoft Visual Studio\2019\Professional\Common7\IDE`)
2. Right click and select "Troubleshot compatibility"
3. Select "Troubleshoot program"
4. Tick "The program requires additional permissions"
5. Click "Next"
6. Press the button "Test the program..."
7. Confirm that VS is launching
8. Click "Next"
9. Select "Yes, save these settings for this program"
10. Click "Close"