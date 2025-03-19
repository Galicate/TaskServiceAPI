# TaskService

TaskService is a roblox API for creating tasks mimicking the roblox engine model.

## Getting Started
A task consists of a task object, and its children of objectives. Tasks control all of the objectives, such as cancelling or activating, while objectives handle progression.

### Creating Tasks
To create a task, you need a name and a list of objectives. 

```
local ServerScriptService = game:GetService("ServerScriptService")
local TaskService = require(ServerScriptService.TaskService)

local newTask = TaskService:Create("Collect Resources", {
	{
		Description = "Collect A Wood Log"
	}
})
```
When creating a task, you can add a few more variables to assist you.
```
TaskService:Create("Collect Resources", {
	{
		Description = "Collect 10 Wood Logs",
		MaxProgress = 10, -- The amount of times needed to progress to complete objective.
		Event = bindableEvent.Event, -- A RBXSignal which progresses the objective when fired.
		Callback = function(resource: BasePart) -- A function to check if the objective should progress when the event fires.
			if resource:HasTag("Wood Log") then return true end
			return false
		end,
	}
})
```
### Activating Tasks
Once your task is created, you must activate it to start tracking progress and triggering events. If a task is not activated, objectives will not progress, and the task cannot be completed.
```
newTask:Activate()
```

### Adding/Removing Players
One of the key features of `TaskService` is that tasks are not player-specific. Multiple players can contribute to the same task simultaneously. You can track contributors using the `Participants` variable and distribute rewards accordingly.
```
newTask:AddPlayers({game.Players.Galicate})
print(newTask.Players) -- Returns {Galicate: Player}
print(newTask.Participants) -- Returns {} (No progress made yet)
```
# Example Script
```
local ServerScriptService = game:GetService("ServerScriptService")
local TaskService = require(ServerScriptService.TaskService)
local Players = game:GetService("Players")

local tasks = {}

Players.PlayerAdded:Connect(function(player: Player)
	local objectives = {
		{
			Description = "Collect 10 Wood Logs",
			MaxProgress = 10,
			Event = ServerScriptService.ItemCollected.Event,
			Callback = function(item: BasePart) 
				if item:HasTag("Wood Log") then return true end
				return false
			end
		}
	}
	local newTask = TaskService:Create("Collect Resources", objectives)
	tasks[player] = newTask
	
	newTask:Activate()
	newTask:AddPlayers({player})
	
	newTask.Completed:Connect(function()
		print(newTask.Players[1].Name .. " collected all of their resources!")
	end)
	
	newTask.Destroyed:Connect(function()
		tasks[player] = nil
	end)
end)

Players.PlayerRemoving:Connect(function(player: Player)
	local task = tasks[player]
	if task then
		tasks[player]:Cancel()
		tasks[player] = nil
	end
end)
```
