For the script, you need to utilize the locale system of `ox_lib`, database library `oxmysql`, the zone system of `ox_lib` as well, and the context menu of `ox_lib`.

There will be private garages: Private garages will be locations where a player approaches the entrance of a garage, and it teleports them inside the IPL. If the player has vehicles in their garage, they need to appear, while you are spawning the vehicles, place the player in a black screen with FadeIn/FadeOut. The vehicles you spawn in the garage (IPL) should be the player's. To have vehicles in their garage, when they arrive at the entrance, they must press E. They simply need to drive the vehicles into their garage. Ensure the player is the vehicle's owner with `owned_vehicles`, and the vehicle is not a blacklisted model (config). If multiple players are in the vehicle, you must bring all the players into the garage (give them all a black screen) and make them appear in the garage (I want you to use buckets to instance players in a garage). There will be a point with a marker (configurable) where the player can leave the garage (if there are guests in the garage, everyone will be expelled from the garage). If the garage owner wants to leave with a vehicle, they just need to get in the vehicle and as soon as they start moving, it will trigger a black screen (Fade Out/Fade In) and spawn them in their vehicle in front of their garage entrance (when the player exits the garage, the vehicle must be created server-side). If multiple players are in a vehicle leaving the garage, the same players must be expelled from the garage and be in the vehicle (and exited from the bucket). There will be a menu accessible to the garage owner (inside the garage) with a marker, when the player is on the marker and presses E it will open the context menu of `ox_lib`. There will be an "Invite People" button, once clicked, retrieve all players near the player's garage entrance, and display the players' names in the submenu, each player will represent a button with the roleplay name of the player as the button name. When clicked, it sends a notification to the player that they have been invited to X's garage and if they want to accept they have 15 seconds (the time is configurable) to press Y (Yes) or N (No). If they press Y, then it will teleport them into the garage/bucket of the player, they will be considered as a guest. When they get into a vehicle (as a driver) they will not be able to move forward or backward, the vehicle will be frozen.

Friendly fire must be disabled by default in private garages between players but can be configurable according to the garage.

Here is an example configuration of a private garage:

```lua
Garages = {}

Garages.Private = {
    {
        name = "Vinewood Garage",
        price = 10000,
        vehiclePositions = {
            -- Assume there are 10 positions available for vehicles
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
        blackScreenDuration = 10, -- in seconds, duration of the black screen when entering/exiting the garage
        invitationTimer = 15, -- in seconds, the time allotted for the player to accept or reject the invitation
        friendlyFireEnabled = false, -- toggles friendly fire on or off
        menuOptions = {
            enabled = true,
            position = vector3(x, y, z),
        },
        garageEntrance = vector3(x, y, z), -- location of the garage entrance
        insideGarage = vector3(x, y, z), -- position inside the garage when on foot
        garageExit = vector4(x, y, z, w), -- exit point from the garage with a vehicle or on foot
    }
}
```

Then there will be so-called public garages: It's the same concept as private garages but these are public garages, we see an NPC, using `ox_target` we right-click on him and click on the option to spawn a vehicle, and we open the menu and the list of our vehicles appears. Each button represents a vehicle if we click on one of the vehicles the player will spawn inside it. To enter a vehicle, you simply need to be next to the NPC and click on the option to enter a vehicle (you must always check if the vehicle belongs to the player).

It is not possible to take out a vehicle if the player already has a vehicle taken out (only applicable for public garages and it's configurable) it is possible to make different public garages here is an example:

```lua

Garages = {}

Garages.Public = {
    {
        name = "Central Parking",
        price = 0,
        vehicleCapacity = 5,
        blacklistedVehicles = {
            ["cargobob"] = true,
            ["police1"] = true
        },
        vehicleExitLimit = 1, -- a player can only take out one vehicle at a time
        purchasePosition = vector3(x, y, z), -- location where the player can buy the garage (indicated by a marker)
        garageMenuPosition = vector3(x, y, z), -- location where the player needs to go to access the menu for viewing and selecting stored vehicles
        markerSettings = {}, -- settings for the garage marker
        vehicleSpawnPosition = vector4(x, y, z, w), -- location where the vehicle will spawn
        vehicleStoragePosition = vector3(x, y, z) -- location where the player needs to go to store the vehicle (close to the spawn position)
    }
}
```

And there will be job garages, same concept as private or public garages, but restricted to specific jobs, a job garage can be private or public but restricted to a specific job.

```lua
Garages = {}

Garages.Jobs = {
    {
        garageType = "private", -- the garage is part of an IPL
        name = "LSPD Garage",
        vehiclePositions = { -- positions for up to 10 vehicles
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
        blackScreenDuration = 10, -- in seconds, duration of black screen while entering/exiting the garage
        invitationTimer = 15, -- in seconds, time to accept or decline the garage invitation
        friendlyFireEnabled = false, -- toggle for friendly fire
        menuOptions = {
            enabled = true,
            position = vector3(x, y, z),
        },
        garageEntrance = vector3(x, y, z), -- entrance to the garage
        insideGarage = vector3(x, y, z), -- inside the garage (on foot)
        garageExit = vector4(x, y, z, w), -- exit from the garage with a vehicle or on foot
        authorizedJobs = {
            ["police"] = true,
            ["sheriff"] = true,
        },
    },
    {
        garageType = "public",
        name = "Government Garage",
        price = 0,
        vehicleCapacity = 5,
        blacklistedVehicles = {
            ["adder"] = true,
            ["t20"] = true
        },
        authorizedJobs = {
            ["government"] = true,
            ["justice"] = true
        },
        vehicleExitLimit = 1, -- a player can only take out one vehicle at a time
        npcSpawnPosition = vector4(x, y, z, w), -- spawn location for the NPC
        vehicleSpawnPosition = vector4(x, y, z, w), -- spawn location for vehicles
    }
}
```

**Edited**
To buy a private garage, you need to create an NPC. When interacting with this NPC and right-clicking on it, you must add an option with ox_target labeled "Buy a garage." Using ox_lib <a href="https://overextended.dev/ox_lib/Modules/Interface/Client/menu">menus</a>, you should open a menu displaying all the private garages available for purchase, including their prices. The name of each button should be the name of the garage. Whenever the player selects a button, you need to create/set a camera inside the specific garage's interior. If the player does not have enough money, you must display a notification.

To buy a public garage, simply visit the designated public garage. For instance, in the example configuration, I've placed a position where you can buy the garage, which should be indicated by a marker. By pressing 'E' at this marker, you can purchase the garage. Afterwards, you can use it by going to the garage position and opening it by pressing 'E'. This will open a menu showing all vehicles stored in your garages. If you select 'enter' on a vehicle, it will spawn with the player inside, and the vehicle will no longer be stored in the garage. Same logic for 
