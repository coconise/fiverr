For the script, you need to utilize the locale system of `ox_lib`, the zone system of `ox_lib` as well, and the context menu of `ox_lib`.

There will be private garages: Private garages will be locations where a player approaches the entrance of a garage, and it teleports them inside the IPL. If the player has vehicles in their garage, they need to appear, while you are spawning the vehicles, place the player in a black screen with FadeIn/FadeOut. The vehicles you spawn in the garage (IPL) should be the player's. To have vehicles in their garage, when they arrive at the entrance, they must press E. They simply need to drive the vehicles into their garage. Ensure the player is the vehicle's owner with `owned_vehicles`, and the vehicle is not a blacklisted model (config). If multiple players are in the vehicle, you must bring all the players into the garage (give them all a black screen) and make them appear in the garage (I want you to use buckets to instance players in a garage). There will be a point with a marker (configurable) where the player can leave the garage (if there are guests in the garage, everyone will be expelled from the garage). If the garage owner wants to leave with a vehicle, they just need to get in the vehicle and as soon as they start moving, it will trigger a black screen (Fade Out/Fade In) and spawn them in their vehicle in front of their garage entrance (when the player exits the garage, the vehicle must be created server-side). If multiple players are in a vehicle leaving the garage, the same players must be expelled from the garage and be in the vehicle (and exited from the bucket). There will be a menu accessible to the garage owner (inside the garage) with a marker, when the player is on the marker and presses E it will open the context menu of `ox_lib`. There will be an "Invite People" button, once clicked, retrieve all players near the player's garage entrance, and display the players' names in the submenu, each player will represent a button with the roleplay name of the player as the button name. When clicked, it sends a notification to the player that they have been invited to X's garage and if they want to accept they have 15 seconds (the time is configurable) to press Y (Yes) or N (No). If they press Y, then it will teleport them into the garage/bucket of the player, they will be considered as a guest. When they get into a vehicle (as a driver) they will not be able to move forward or backward, the vehicle will be frozen.

Friendly fire must be disabled by default in private garages between players but can be configurable according to the garage.

Here is an example configuration of a private garage:

```lua
Garages = {}

Garages.Private = {
    {
        name = "Vinewood Garage",
        price = 10000,
        vehiclesPlaces = {
            -- positions of vehicles, let's assume we have 10 places here
            vector4(x, y, z, w),
            vector4(x, y, z, w),
            vector4(x, y, z, w),
            vector4(x, y, z, w),
            vector4(x, y, z, w),
            vector4(x, y, z, w),
            vector4(x, y, z, w),
            vector4(x, y, z, w),
            vector4(x, y, z, w),
            vector4(x, y, z, w)
        },
        blacklistedVehicles = {
            ["cargobob"] = true,
            ["police1"] = true
        },
        blackScreenTime = 10, -- in seconds, how much time the player is in blackscreen before going in/out of the garage
        timerInvite = 15, -- in seconds, the time the player needs to say yes or no to accept the invite
        friendlyfire = false, -- enable friendly fire or not
        menuGarage = {
            enable = true,
            position = vector3(x, y, z),
        },
        garageEnter = vector3(x, y, z), -- enter of the garage
        insideGarage = vector3(x, y, z), -- inside of the garage (when on foot)
        leaveGarage = vector4(x, y, z, w), -- when leaving the garage with a vehicle or at foot
    }
}
```

Then there will be so-called public garages: It's the same concept as private garages but these are public garages, we see an NPC, using `ox_target` we right-click on him and click on the option to spawn a vehicle, and we open the menu and the list of our vehicles appears. Each button represents a vehicle if we click on one of the vehicles the player will spawn inside it. To enter a vehicle, you simply need to be next to the NPC and click on the option to enter a vehicle (you must always check if the vehicle belongs to the player).

It is not possible to take out a vehicle if the player already has a vehicle taken out (only applicable for public garages and it's configurable) it is possible to make different public garages here is an example:

```lua

Garages = {}

Garages.Public = {
    {
        name = "Parking Central",
        price = 0,
        vehiclesPlaces = 5,
        blacklistedVehicles = {
            ["cargobob"] = true,
            ["police1"] = true
        },
        limitVehicleOut = 1, -- the player can only out of the garage 1 vehicle
        NPCSpawn = vector4(x, y, z, w), -- where does the NPC going to spawn
        coordsSpawn = vector4(x, y, z, w), -- where does the vehicle going to spawn?
    }
}
```

And there will be job garages, same concept as private or public garages, but restricted to specific jobs, a job garage can be private or public but restricted to a specific job.

```lua
Garages = {}

Garages.Jobs = {
    {
        garageType = "private", -- the garage going to be into a IPL
        name = "LSPD Garage",
        vehiclesPlaces = {
         -- positions of vehicles, let's assume we have 10 places here
            vector4(x, y, z, w),
            vector4(x, y, z, w),
            vector4(x, y, z, w),
            vector4(x, y, z, w),
            vector4(x, y, z, w),
            vector4(x, y, z, w),
            vector4(x, y, z, w),
            vector4(x, y, z, w),
            vector4(x, y, z, w),
            vector4(x, y, z, w)
        },
        blacklistedVehicles = {
            ["adder"] = true,
            ["t20"] = true
        },
        blackScreenTime = 10, -- in seconds, how much time the player is in blackscreen before going in/out of the garage
        timerInvite = 15, -- in seconds, the time the player needs to say yes or no to accept the invite
        friendlyfire = false -- enable friendly fire or not
        menuGarage = {
            enable = true,
            position = vector3(x, y, z),
        },
        garageEnter = vector3(x, y, z), -- enter of the garage
        insideGarage = vector3(x, y, z), -- inside of the garage (when on foot)
        leaveGarage = vector4(x, y, z, w), -- when leaving the garage with a vehicle or at foot
        jobAuthorized = {
            ["police"] = true,
            ["sheriff"] = true,
        },
    },
    {
        garageType = "public"
        name = "Governement Garage",
        price = 0,
        vehiclesPlaces = 5,
        blacklistedVehicles = {
            ["adder"] = true,
            ["t20"] = true
        },
        jobsAuthorized = {
            ["government"] = true,
            ["justice"] = true
        }
        limitVehicleOut = 1, -- the player can only out of the garage 1 vehicle
        NPCSpawn = vector4(x, y, z, w), -- where does the NPC going to spawn
        coordsSpawn = vector4(x, y, z, w), -- where does the vehicle going to spawn?
    }
}
```