---
layout: post
title:  "running powershell scripts against azure through visual studio team services"
date:   2018-09-23 13:56:00 +0100
categories: [bigyak, azure, vsts, devops, powershell]
---
I had a need to run some PowerShell scripts that made use of the [Azure PowerShell cmdlets](https://docs.microsoft.com/en-us/powershell/azure/overview) on [the tool formerly known as Visual Studio Team Services](https://visualstudio.microsoft.com/team-services/). I initially tried using the PowerShell task, but had great difficulty getting it to authenticate properly to Azure. I did a lot of searching, and eventually found out that the answer was really easy.

### Pre-requisites
You'll need the following things ready to follow through:
1. A build or release pipeline. I've set up a blank release pipeline for this post.
![Blank pipeline](/assets/2018-09-23-emptypipeline.png)

2. A PowerShell script that you want to run which contains some Azure cmdlets.
The demo script I'm going to use simply logs a list of virtual machines in the subscription. note that I don't need to do anything inside the script to login to Azure - we'll take care of that in the pipeline.

```
    $VM = Get-AzureRmVm 
    Write-Output $VM
```

### Step by step
The first thing we need to do is create an Azure Service Principal, which is essentially an account in your Azure Active Directory that VSTS/AzDO with use to login to Azure.
Go to the project settings menu (the little cog) and click Services.
![Settings menu](/assets/2018-09-23-settingsmenu.png)

In the Services page, click New Service Connection, and select Azure Resource Manager (you can also select Azure Classic if you need to use the classic cmdlets).
![New service connection](/assets/2018-09-23-newserviceconnectionmenu.png)

Give the service connection a suitable name, and optionally select a resource group to restrict it to if that's useful for you. Click OK.
![Add Azure service connection](/assets/2018-09-23-addazsp.png)

You should now be able to see your new service connection in the list on the left.
![Service principal list](/assets/2018-09-23-splist.png)

Back in your pipeline, add a task and select Azure PowerShell from the list. This differs from the PowerShell task in that it lets us make use of the service connection that we've set up to automatically authenticate to Azure. Note that this still runs on the VSTS agent - it is not a Cloud Shell instance.
![Add Azure PowerShell task](/assets/2018-09-23-addazps.png)

In the task options, select your new service connection from the Azure Subscription drop-down and your script to run from the Script Path control. You can also specify which version of Azure PowerShell you need to run; I don't need any old behaviour, so I've just gone with Latest installed version.
![Azure PowerShell options](/assets/2018-09-23-azpsoptions.png)

Finally, save and run your pipeline. You can then take a look in the logs and see that the script has run succesfully and given us the output we wanted!
![VSTS logs](/assets/2018-09-23-log.png)

