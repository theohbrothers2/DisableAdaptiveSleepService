# DisableAdaptiveSleepService
Keeps AdaptiveSleepService in Manual Startup state and Stopped, across Windows Updates or driver updates.

# Background
Since Windows build 10586 onwards, an AMD graphics driver update installed either manually (AMD's website) or through Windows Updates includes a service called AdaptiveSleepService. This Service however causes PCs in Sleep states to engage in Hibernate-from-Sleep, often appearing like a brief waking from Sleep and going back to Sleep. 

The solution would be to disable AdaptiveSleepService. However, because Windows Updates also includes AMD graphics drivers, the problem soon recurs. 

<b>The full-fledged solution would be to automate the disabling of AdaptiveSleepService.</b>

# How to
We are going to create a Task in Task Scheduler, that runs the .bat script on 1) WorkStation Lock, 2) When a system enters Sleep (EventID 42 of Kernel-Power Log). Because a workstation lock occurs before a system enters Sleep, it will trigger the Task.
- Download the <code>DisableAdaptiveSleepService.bat</code> to <code>C:\Scripts\DisableAdaptiveSleepService.bat</code>
- Download the <code>DisableAdaptiveSleepService.xml</code> Task Export to <code>C:\Scripts\DisableAdaptiveSleepService.xml</code>.
- Import the <code>DisableAdaptiveSleepService.xml</code> Task Export file as a Task in <i>Task Scheduler</i>. Click <i>OK</i> when the Create Task Wizard Pops up.
  - Full instructions: Open <i>Task Scheduler</i>, create a new folder in Task Scheduler Library called <i>scripts</i>, then in that folder click on <i>Import Task</i>, browse to <code>C:\scripts\DisableAdaptiveSleepService.xml</code> to import the Task. Click <i>OK</i> when the Create Task Wizard pops up.

# Test the script (it should work!)
Open Service, and change AdaptiveSleepService startup type to Automatic, and Start the service. Now Sleep your system. Now wake your system, and see now <code>AdaptiveSleepService</code> <i>Startup Type</i> should be changed to <code>Manual</code> and <i>Status</i> should be <code>Stopped</code>. :)

# Note
- Task export has hardcoded the path of the script to be to be run in C:\scripts\DisableAdaptiveSleepService.bat. Change this to your liking.
- Task export runs the script under SYSTEM user, but with least privileges. Change this to your user with administrative rights. However, note that EventID Task triggers do not work for non-SYSTEM users (or at least it appears to be from my experience).

# Platforms
- WinNT (Windows Vista and Up)

