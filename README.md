# customdisplayprofiles

This is a simple command-line Python script that can check, set or unset a custom ColorSync ICC profile for a given display. It uses PyObjC and the most current (as of 2013) ColorSync API to do this.

## Usage

### Setting a profile

Use the `set` action to set a profile (as the user running the command) for the main display.

`customdisplayprofiles set /path/to/profile.icc`

Use the `--display` option to configure an alternate display.

`customdisplayprofiles set --display 2 /path/to/profile.icc`

If you want to get a list of displays with their associated index:

`customdisplayprofiles displays`

### Configurable user scope

The `--user-scope` option allows you to define whether the profile will be applied to the "Current" or "Any" user domain, which may allow you set this preference as a default for all users:

`customdisplayprofiles set --user-scope any /path/to/profile.icc`

Specifying `any` here requires root privileges, as it will write these preferences to a system-owned location.

More information on the user preferences system on OS X can be found [here](https://developer.apple.com/library/mac/#documentation/userexperience/Conceptual/PreferencePanes/Concepts/Managing.html).

### Retrieving the current profile

The full path to an ICC profile can be printed to stdout:

`customdisplayprofiles current-path`

This could be useful if you want to check the current setting using an idempotent login script or a configuration framework like Puppet.

`current-path` will output nothing if there is no profile currently set for that display.

### Full details

A more complete dictionary of information can be printed with the `info` action:

<pre><code>
➜ ./customdisplayprofiles info
{
    CustomProfiles =     {
        1 = "file://localhost/Library/Application%20Support/Adobe/Color/Profiles/SMPTE-C.icc";
    };
    DeviceClass = mntr;
    DeviceDescription = iMac;
    DeviceHostScope = kCFPreferencesCurrentHost;
    DeviceID = "<CFUUID 0x7fb6204abea0> 00000610-0000-B005-0000-0000042C0140";
    DeviceUserScope = kCFPreferencesAnyUser;
    FactoryProfiles =     {
        1 =         {
            DeviceModeDescription = iMac;
            DeviceProfileURL = "file://localhost/Library/ColorSync/Profiles/Displays/iMac-00000610-0000-B005-0000-0000042C0140.icc";
        };
        DeviceDefaultProfileID = 1;
    };
}
</pre></code>