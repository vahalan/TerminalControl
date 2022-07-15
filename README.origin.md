Original code from: https://www.codeproject.com/articles/20781/terminal-control-library-c-vt100-ansi-xterm-ssh-te

-----

# Introduction
My original project can be found here.

I've been trying for a while to come up with a .NET control that would handle VT100/ANSI formatting and connect to SSH2 servers.

I've been through the commercial ones and still haven't found one that suited me.

My last project was based on Granados SSH library (open source SSH .NET implementation) and AckTerm (open source partially completed VT100 control).

# Background
This library is an attempt at creating a library based on the "Poderosa" project which can be found here.

Poderosa IMO is the best terminal emulator. It's open source, and is better than the commercial clients. Go check it out for yourself, it's worth it. You can find the original Poderosa CSV source there.

I tried for a couple of weeks to encapsulate components of Poderosa, but the code comments are all in Japanese and I couldn't find any good docs that described the source code. The developer community for Poderosa also appears to be all Japanese, so I felt pretty much on my own.

# Using the Code
What I pretty much did here was encapsulate the entire Poderosa application into a library. Then I exposed certain elements so an application developer can simply drop the "Terminal Control.dll" into his/her app and have a control that acts like you would expect any .NET control to act.

This code was written using Visual Studio .NET 2005 Professional and requires .NET Framework 2.0.

# Points of Interest
The Poderosa solution is made up of 5 projects:

- Container - This is the actual Poderosa application
- Terminal - A library providing Telnet capabilities as well as the Terminal Emulation
- Common - Code that is shared between the Poderosa application and the Port Forwarding Tool
- Granados - SSH library that supports just about every feature of SSH
- Portforwarding - A standalone application (uses Terminal, Common, and Granados libraries) that provides SSH portforwarding functionality

# First Step
What I've done with my library, was to change "Container" into a Library output, and implement a public class based on System.Windows.Forms.Control.

# Second Step
The first time my class ("WalburySoftware.TerminalControl") is instanced, it runs Poderosa code:

## C#

```C#
GApp.Run(args); GApp._frame._multiPaneControl.InitUI(null, GApp.Options);
GEnv.InterThreadUIService.MainFrameHandle = GApp._frame.Handle; 
```

Poderosa has lots and lots and lots of static variables. Until I can get the time and motivation to go through them and make the appropriate changes, these variables will need to be initialized before you can use any of Poderosa's classes.

# Third Step
If you look at the method Poderosa.ConnectionCommandTarget.Resize, you'll see the line:

## C#
```C#
if(GEnv.Connections.FindTag(_connection).ModalTerminalTask!=null) 
	return CommandResult.Denied 
```
This throws an exception. I took it out because it doesn't do anything for my library. I'm not sure what the ModalTerminalTask variable is used for. If you include that line, the terminal will not resize properly.

# Final Step
Now we are free to create and use TerminalPanes as we need.

You'll see the steps in WalburySoftware.TerminalControl.Connect() that connect the TerminalPane to an SSH2 connection.

# License
This article, along with any associated source code and files, is licensed under The Code Project Open License (CPOL)

# About the Author

Columbus-MCSD

