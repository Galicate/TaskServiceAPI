# TaskService

# Summary
`Service` `Server-Side`

TaskService allows developers to create and interact with task objects in a global way. It may be more convenient to listen to events through this service rather than on individual task objects.

# Methods

## Create
This constructor creates a new Task from 2 arguments: the name of the task, and a list of objectives to include. 

### Code Sample
```
local ServerScriptService = game:GetService("ServerScriptService")
local TaskService = require(ServerScriptService.TaskService)

local newTask = TaskService:Create("Collect Resources", {
	{
		Description = "Collect A Wood Log"
	}
})
```

# Events

## TaskActivated
This event triggers when the task is activated.

## TaskCompleted
This event triggers when the task is completed.

## TaskCancelled
This event triggers when the task is cancelled.

## TaskDestroyed
This event triggers when the task is destroyed.

## ObjectiveProgressed
This event triggers when the objective progresses.

## ObjectiveCompleted
This event triggers when the objective is completed.

