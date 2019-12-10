---
title: Azure ARM Templates with Azure Powershell CLI
categories:
- Azure
exerpt: "Using ARM Templates together with the Azure Powershell CLI we can reduce the amount of manual configuration or setup we have to do in an Azure subscription."
---

Using ARM Templates together with the Azure Powershell CLI we can reduce the amount of manual configuration or setup we have to do in an Azure subscription.

***Background***

When setting up alerts in Azure, we can use [Azure Alerts](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-overview) which is part of Azure Monitor. Doing this in the Azure Portal is fairly simple, one needs to create an Action Group (contianing the email or endpoint to call when the alert triggers), and the alert criteria.

***Reason for Automation***

Granted there does not need to be a reason to automate, since that should be the de-facto standard in today's world, but within the context of Azure alerts, I recently had to setup alerts for an Azure subscription that had many different resources, like Azure Virtual Machines, Azure SQL Databases, multiple Local Network Gateways etc. One of the other things I needed to consider is that there will be other subscriptions which this needs to be done for as well, and the resources and multiplicity of them are unknown (subscription A will have 1xVM, 2xSQL Databases whereas subscription B will have 10xVMs and 1xSQL Database).

Defining ARM Templates for the Azure Alerts and using the Azure Powershell CLI to discover the resources in the subscription and deploy the ARM Templates based on those resources seems the perfect approach for this king of situation.

***ARM Template for Azure Alerts***

One way to get the ARM Template for any resource is to manually deploy the resource into Azure, then on the resource blade, we can get the template by going to **Export Template**

<img src="https://github.com/RonaldMariah/ronaldmariah.github.io/raw/master/assets/azure-arm-powershell/export-template.JPG" />

We can then save this template locally and edit it, so that it is configurable. 

***Making the ARM Template configurable***

Making an ARM template configurable involves creating parameters that can be passed into the template. Within the context of the Azure alerts, we can create parameters for the Action Group, the Resource ID, the Alert's name and description and various other properties which we want to keep configurable.

An example of a configurable ARM template for Azure alerts is the following. This template can be used for Azure alerts that trigger based on [Metrics](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-metric-overview).

Save the following file as **alert_deployment.json**

```
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "alertNamePrefix": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Alert Prefix"
            }
        },
        "alertNameSuffix": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Alert Suffix"
            }
        },
        "alertDescription": {
            "type": "string",
            "defaultValue": "This is a metric alert",
            "metadata": {
                "description": "Description of alert"
            }
        },
        "alertSeverity": {
            "type": "int",
            "defaultValue": 3,
            "allowedValues": [
                0,
                1,
                2,
                3,
                4
            ],
            "metadata": {
                "description": "Severity of alert {0,1,2,3,4}"
            }
        },
        "resourceId": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Full Resource ID of the resource emitting the metric that will be used for the comparison. For example /subscriptions/00000000-0000-0000-0000-0000-00000000/resourceGroups/ResourceGroupName/providers/Microsoft.compute/virtualMachines/VM_xyz"
            }
        },
        "metricName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the metric used in the comparison to activate the alert."
            }
        },
        "operator": {
            "type": "string",
            "defaultValue": "GreaterThan",
            "allowedValues": [
                "Equals",
                "NotEquals",
                "GreaterThan",
                "GreaterThanOrEqual",
                "LessThan",
                "LessThanOrEqual"
            ],
            "metadata": {
                "description": "Operator comparing the current value with the threshold value."
            }
        },
        "threshold": {
            "type": "string",
            "defaultValue": "0",
            "metadata": {
                "description": "The threshold value at which the alert is activated."
            }
        },
        "timeAggregation": {
            "type": "string",
            "defaultValue": "Average",
            "allowedValues": [
                "Average",
                "Minimum",
                "Maximum",
                "Total",
                "Count"
            ],
            "metadata": {
                "description": "How the data that is collected should be combined over time."
            }
        },
        "windowSize": {
            "type": "string",
            "defaultValue": "PT5M",
            "allowedValues": [
                "PT1M",
                "PT5M",
                "PT15M",
                "PT30M",
                "PT1H",
                "PT6H",
                "PT12H",
                "PT24H"
            ],
            "metadata": {
                "description": "Period of time used to monitor alert activity based on the threshold. Must be between one minute and one day. ISO 8601 duration format."
            }
        },
        "evaluationFrequency": {
            "type": "string",
            "defaultValue": "PT1M",
            "allowedValues": [
                "PT1M",
                "PT5M",
                "PT15M",
                "PT30M",
                "PT1H"
            ],
            "metadata": {
                "description": "how often the metric alert is evaluated represented in ISO 8601 duration format"
            }
        },
        "actionGroupId": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The ID of the action group that is triggered when the alert is activated or deactivated"
            }
        },
        "metricNamespace": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The namespace of the metric."
            }
        }
    },
    "variables": {  },
    "resources": [
        {
            "name": "[concat(parameters('alertNamePrefix'), parameters('alertNameSuffix'))]",
            "type": "Microsoft.Insights/metricAlerts",
            "location": "global",
            "apiVersion": "2018-03-01",
            "tags": {},
            "properties": {
                "description": "[parameters('alertDescription')]",
                "severity": "[parameters('alertSeverity')]",
                "enabled": true,
                "scopes": ["[parameters('resourceId')]"],
                "evaluationFrequency":"[parameters('evaluationFrequency')]",
                "windowSize": "[parameters('windowSize')]",
                "criteria": {
                    "odata.type": "Microsoft.Azure.Monitor.SingleResourceMultipleMetricCriteria",
                    "allOf": [
                        {
                            "name" : "1st criterion",
                            "metricName": "[parameters('metricName')]",
                            "dimensions":[],
                            "operator": "[parameters('operator')]",
                            "threshold" : "[parameters('threshold')]",
                            "timeAggregation": "[parameters('timeAggregation')]",
                            "metricNamespace": "[parameters('metricNamespace')]"
                        }
                    ]
                },
                "actions": [
                    {
                        "actionGroupId": "[parameters('actionGroupId')]"
                    }
                ]
            }
        }
    ]
}
```

Once we have the configurable ARM template, we not need to deploy it and pass in the parameters we want. To do this we can use the Azure Powershell to script the discovery of the resources and deploy the ARM template.

The first thing we need to do in the powershell script is to login to Azure.
We can do this with the following command

```
Login-AzureRmAccount
```

Next lets create a Resource Group which we will use to deploy the Azure Alerts into

```
New-AzureRmResourceGroup -Name alerts-rg -Location "South Africa North"
```

Next we need to get a list of all the Virtual Machines in the subscription.
We can save it in a variable so that we can use it later on.

```
$virtualMachines = Get-AzureRmVM
```

Once we get the list of VMs, we can iterate over them and deploy the alerts for high CPU like the following

**The action group will need to be created beforehand and the Action group Id can be used in the following snippet**

```
foreach($virtualMachine in $virtualMachines){ `
    $description = "Virtual Machine CPU High - $($virtualMachine.Name)"
    New-AzureRmResourceGroupDeployment `
        -Name Deployment `
        -ResourceGroupName alerts-rg `
        -TemplateFile alert_deployment.json `
        -alertNamePrefix "azure-alerts" `
        -alertNameSuffix "-high-cpu-$($virtualMachine.Name)" `
        -alertDescription $description `
        -alertSeverity 1 `
        -resourceId $virtualMachine.Id `
        -metricName "Percentage CPU" `
        -operator "GreaterThan" `
        -threshold 80 `
        -timeAggregation "Average" `
        -metricNamespace "microsoft.compute/virtualmachines" `
        -evaluationFrequency "PT5M" `
        -windowSize "PT5M" `
        -actionGroupId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/XXX/providers/microsoft.insights/actiongroups/XXX"
}
```

Once the Powershell script is complete, we can then run it and this will discover the VMs in the subscription and deploy ARM templates for the Azure Alerts for High CPU.

A similar approach can be take for other alerts as well, like High Memory, and also other resources (High DTU for SQL Databases).

This ends this post!
Thanks!

Let me know what you think, and other ways we can automate using Azure!