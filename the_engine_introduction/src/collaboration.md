## Collaboration

The editor has built-in support for real-time collaboration, allowing multiple people to work
together in the same project. All user actions — importing assets, creating and editing entities,
etc, are supported in the collaborative workflow.

In our collaboration model, one of the users always acts as a host. The host
invites others to join her in editing *her* project. All the changes made by the
participants in the session end up in the host’s project and it’s the host’s
responsibility to save the project, check in the changes into version control, or
do whatever else is needed to make the changes permanent.

To host a collaboration setting, open the collaboration tab from **Tab >
Collaboration** and choose one of the **Host** options.

* **Host LAN Server**: Host a server on your LAN. The system will choose a free port on your machine
  for hosting. Other users on the same LAN can join your session by choosing *Join LAN Server* and
  selecting your machine.

* **Host Internet Server**: Host a server that can be accessed over the internet on a specified port.
  If you check the "Use UPnP" checkbox the system will attempt to use UPnP to open the port in your
  router, so that external users can access your server. To connect, an external user would choose
  *Join Internet Server* and specify your external IP address.

  Note that there is no guarantee that UPnP works with your particular router. If Internet hosting
  is not working for you, you may have to manually forward the hosting port in your router.

* **Host Discord Server**: Host a server through Discord. You must have the Discord app running on
  your machine to use this option. You can invite friends to your discord editing session by selecting
  them in your active friends list and sending them invites.

![Collaboration tab when hosting.](https://www.dropbox.com/s/md2plg22dfa652u/collaboration-tab.png?dl=1)

If you just want to try out collaboration on your own, you can run the client and the host on the
same machine (just start two instances of the_machinery.exe) and connect using the LAN option.

![Collaboration session running on a single machine.](https://www.dropbox.com/s/34a8328lt19rqpr/collaboration-session.png?dl=1)
