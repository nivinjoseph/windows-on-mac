# Background:

As a developer, you might want to do windows development on a MacBook Pro. While installing Windows on one is fairly easy using Bootcamp, getting a Windows dev environment going with virtualization support (which is a requirement for Docker) can be excruciatingly painful. The purpose of this document is to outline the steps required to alleviate the pain. This applies to newer MacBook Pros (>= 2016, might be even earlier).

Alternate methods for running a Windows dev env on a MacBook Pro exist. For example, running windows inside Parallels inside OSX. Methods like the aforementioned are sub-optimal for various reasons including performance. Also in the case of Parallels, the nested virtualization supported by the product does not play well with Docker. An approach to overcome this issue might be to not run Docker in Windows, but instead in OSX and have apps running in Windows connect to the containers running in OSX. However, you will still pay a performance penalty and that is not ideal.

# Prerequisites:

- MacBook Pro (>= 2016)
- Patience and attention to detail

# Details:

Execute the following steps to achieve the desired outcome.

1. Boot into OSX
2. Download the latest Windows 10 image
3. Start Bootcamp from OSX and follow the instructions. After Bootcamp is done its thing, you should be able to boot into Windows.
4. Boot into OSX.
5. Follow the instructions by this dude available at this [link](http://nuts4.net/post/hack-force-vt-x-to-be-always-on-when-booting-to-windows-on-your-macbook). He is smart. Read the details there so you know what is going on and what you have to do. This step is the most complicated step.
6. Boot into Windows. This must be a clean setup. As in, Docker is not installed and neither are the windows features Hyper-v and Containers. If any of them are already installed, get rid of them.
7. Open PowerShell as admin and enter this command. 
`Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All`
8. Reboot the machine and get back into windows.
9. Open PowerShell as admin and enter this command. 
`Enable-WindowsOptionalFeature -Online -FeatureName Containers -All` No need to reboot
10. Open PowerShell as admin and enter this command 
`MOFCOMP %SYSTEMROOT%\System32\WindowsVirtualization.V2.mof`
11. Reboot the machine and get back into Windows.
12. Now install Docker for Windows and then reboot if asked to do so and get back into Windows.

At this point Docker should be able to launch and function properly.