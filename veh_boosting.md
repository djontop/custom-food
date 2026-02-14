# DJ Vehicle Boosting System - Complete Documentation

## Table of Contents
1. [Introduction](#introduction)
2. [Features](#features)
3. [Installation](#installation)
4. [Configuration](#configuration)
5. [Database Setup](#database-setup)
6. [System Integration](#system-integration)
7. [Usage Guide](#usage-guide)
8. [API Reference](#api-reference)
9. [Troubleshooting](#troubleshooting)
10. [Support & Credits](#support--credits)

---

## Introduction

**DJ Vehicle Boosting System** is a comprehensive FiveM script for QBCore Framework that provides an immersive vehicle theft and delivery mission system. Players can accept contracts to steal high-value vehicles, evade police GPS tracking, and deliver them to dealers for rewards and reputation progression.

### Key Highlights
- **15-Level Reputation System** with XP progression
- **90+ Vehicles** across different difficulty tiers
- **Dynamic GPS Tracking** with police alerts
- **Vehicle Damage System** with chopping mechanics
- **Leaderboard & Statistics** tracking
- **Shop System** with custom currency
- **Multiple Phone System** integrations
- **Discord Logging** support

---

## Features

### Mission System
- ✅ Random vehicle spawn locations (40+ locations)
- ✅ NPC guards protecting target vehicles
- ✅ GPS tracker system with police notifications
- ✅ Hacking minigame to disable trackers
- ✅ Timed missions with countdown
- ✅ Random or fixed delivery locations
- ✅ Dealer NPC with walkaway animation
- ✅ Global and personal cooldowns

### Progression System
- ✅ 15 reputation levels
- ✅ XP-based progression
- ✅ Level-locked vehicle tiers
- ✅ Increasing rewards per level
- ✅ Statistics tracking (missions, earnings, success rate)
- ✅ Mission history (last 20 missions)
- ✅ Global leaderboard (top 10 players)

### Combat & Challenge
- ✅ Configurable NPC guards (health, armor, weapons)
- ✅ Force kill mode (bullet-only damage)
- ✅ Vehicle impact protection for NPCs
- ✅ Realistic combat AI

### Vehicle Mechanics
- ✅ Vehicle damage checking
- ✅ Chopping system for damaged vehicles
- ✅ Reduced rewards for chopped vehicles
- ✅ Destruction animation option
- ✅ Auto fuel refill on spawn

### Economy
- ✅ Custom currency (Nitro Cash)
- ✅ In-game shop with materials
- ✅ Quantity purchasing system
- ✅ Configurable item prices

### Police Integration
- ✅ Real-time GPS tracking
- ✅ Automatic alerts to on-duty police
- ✅ Tracker update intervals
- ✅ Multiple police job support

### System Compatibility
- ✅ Auto-detection for inventory systems (ox_inventory, qb-inventory)
- ✅ Auto-detection for target systems (ox_target, qb-target)
- ✅ Auto-detection for fuel systems (ox_fuel, lc_fuel, LegacyFuel)
- ✅ Phone system integration (gksphone, qs-smartphone, qb-phone, yseries)
- ✅ ox_lib UI components (notifications, progress bars, skillcheck)
- ✅ Fallback to QBCore alternatives

### Additional Features
- ✅ Discord webhook logging
- ✅ Config validation system
- ✅ Debug commands
- ✅ Comprehensive error handling
- ✅ Event name obfuscation

---

## Installation

### Prerequisites
- QBCore Framework (latest version)
- oxmysql or mysql-async
- ox_lib (for UI components)
- One of the supported inventory systems
- One of the supported target systems

### Step-by-Step Installation

1. **Download the Script**
   ```bash
   # Extract to your resources folder
   [your-server]/resources/[custom]/dj_boosting/
   ```

2. **File Structure**
   ```
   dj_boosting/
   ├── server.lua
   ├── client.lua
   ├── config.lua
   ├── fxmanifest.lua
   └── README.md
   ```

3. **Add to server.cfg**
   ```lua
   ensure ox_lib
   ensure dj_boosting
   ```

4. **Database Setup**
   - The script **automatically creates** required tables on first start
   - Tables: `boosting_players` and `boosting_history`
   - No manual SQL execution needed

5. **Configure Items**
   Add these items to your `qb-core/shared/items.lua`:
   ```lua
   -- Hacking Device
   ['nitrohack'] = {
       ['name'] = 'nitrohack',
       ['label'] = 'Nitro Hack Device',
       ['weight'] = 500,
       ['type'] = 'item',
       ['image'] = 'nitrohack.png',
       ['unique'] = false,
       ['useable'] = false,
       ['shouldClose'] = true,
       ['description'] = 'Advanced device to disable GPS trackers'
   },

   -- Currency (example using steel as currency)
   ['steel'] = {
       ['name'] = 'steel',
       ['label'] = 'Steel',
       ['weight'] = 100,
       ['type'] = 'item',
       ['image'] = 'steel.png',
       ['unique'] = false,
       ['useable'] = false,
       ['shouldClose'] = true,
       ['description'] = 'Hardened alloy material'
   },

   -- Hack Item (example using plastic)
   ['plastic'] = {
       ['name'] = 'plastic',
       ['label'] = 'Plastic',
       ['weight'] = 50,
       ['type'] = 'item',
       ['image'] = 'plastic.png',
       ['unique'] = true,
       ['useable'] = true,
       ['shouldClose'] = true,
       ['description'] = 'Used for hacking vehicle trackers'
   },
   ```

6. **Add Item Images**
   Place item images in:
   - **ox_inventory**: `ox_inventory/web/images/`
   - **qb-inventory**: `qb-inventory/html/images/`

7. **Restart Server**
   ```bash
   restart dj_boosting
   ```

---

## Configuration

### config.lua Overview

#### System Detection
```lua
-- Auto-detect or force specific systems
Config.InventorySystem = nil  -- nil = auto, "ox_inventory", "qb-inventory"
Config.TargetSystem = nil     -- nil = auto, "ox_target", "qb-target"
Config.FuelSystem = nil       -- nil = auto, "ox_fuel", "lc_fuel", "LegacyFuel"
Config.PhoneSystem = nil      -- nil = auto, "gksphone", "qs-smartphone", "qb-phone", "yseries"
```

#### UI Preferences
```lua
Config.UseOxLibNotifications = true  -- Use ox_lib notifications
Config.UseOxLibProgress = true       -- Use ox_lib progress bars
Config.UseOxLibSkillcheck = true     -- Use ox_lib skillcheck minigame
```

#### Mission Settings
```lua
Config.MissionTimer = 20              -- Mission duration in minutes
Config.GlobalCooldown = 300000        -- 5 minutes in milliseconds
Config.PersonalCooldown = 300000      -- 5 minutes in milliseconds
Config.TrackerUpdateInterval = 3000   -- Police tracker update (3 seconds)
Config.HackAttempts = 3               -- Max hack attempts before mission fails
```

#### Vehicle Damage System
```lua
Config.CheckVehicleDamage = true      -- Enable damage checking
Config.MinEngineHealth = 300          -- Minimum engine health (0-1000)
Config.MinBodyHealth = 500            -- Minimum body health (0-1000)
Config.AllowChopDamaged = true        -- Allow chopping damaged vehicles
Config.ChopRewardPercent = 30         -- Percent of reward when chopping
Config.ChopAnimation = true           -- Show destruction animation
```

#### NPC Combat Settings
```lua
Config.ForceNPCKill = true            -- NPCs can only be killed by bullets
Config.DisableVehicleKills = true     -- Prevent vehicle impact kills
Config.NPCModel = 'g_f_y_lost_01'     -- NPC model
Config.NPCWeapon = 'WEAPON_PISTOL'    -- NPC weapon
Config.NPCCount = 4                   -- Number of guards
Config.NPCHealth = 150                -- Min health
Config.NPCHealthMax = 200             -- Max health
Config.NPCAccuracy = 25               -- Accuracy (0-100)
Config.NPCCombatAbility = 50          -- Combat ability (0-100)
Config.NPCArmour = 0                  -- Armour value
Config.NPCSpreadRadius = 15.0         -- Spread radius around vehicle
```

#### Leveling System
```lua
Config.XPPerLevel = 100               -- XP needed per level
Config.MaxLevel = 15                  -- Maximum reputation level
```

#### Items
```lua
Config.HackItem = 'plastic'           -- Item used for hacking
Config.CurrencyItem = 'steel'         -- Custom currency item
```

#### Police Jobs
```lua
Config.PoliceJobs = {'police', 'sheriff', 'state'}
```

#### NPC Locations
```lua
-- Boosting Contract NPC
Config.BoostingPed = {
    coords = vector4(-185.69, -1317.81, 31.30, 180.0),
    model = 'ig_g',
    scenario = 'WORLD_HUMAN_SMOKING',
}
```

#### Delivery Settings
```lua
Config.UseRandomDelivery = true  -- Random locations with NPC dealer

-- Fixed delivery (when UseRandomDelivery = false)
Config.DeliveryLocation = vector3(954.28, -138.85, 74.39)

-- Random delivery locations (when UseRandomDelivery = true)
Config.DeliveryLocations = {
    {
        coords = vector4(-1884.676, -658.116, 10.26, 222.049),
        label = 'Mirror Park Garage'
    },
    -- Add more locations...
}
```

#### Dealer NPC Settings
```lua
Config.DealerNPC = {
    model = 'ig_dale',
    walkawayScenario = 'WORLD_HUMAN_SMOKING',
    walkawayDelay = 3000,
    walkawayDistance = 50.0,
}
```

#### Vehicle Spawn Locations
40+ spawn locations across the map
```lua
Config.SpawnLocations = {
    vector4(-2480.9, -212.0, 17.4, 0.0),
    -- ... 40+ more locations
}
```

#### Vehicle Tiers (15 Levels)
```lua
Config.VehiclesByLevel = {
    ['1'] = {  -- Level 1 vehicles
        {model = 'tailgater', name = 'Tailgater', reward = 50, xp = 5},
        -- ... more vehicles
    },
    ['15'] = {  -- Level 15 vehicles
        {model = 'krieger', name = 'Benefactor Krieger', reward = 1900, xp = 100},
        -- ... more vehicles
    },
}
```

#### Shop Items
```lua
Config.ShopItems = {
    {
        label = 'Nitro Hack Device',
        name = 'nitrohack',
        price = 50,
        description = 'Advanced hacking device'
    },
    -- ... more items
}
```

---

## Database Setup

### Automatic Table Creation
The script automatically creates required tables on first startup:

#### boosting_players Table
```sql
CREATE TABLE IF NOT EXISTS `boosting_players` (
    `id` int(11) NOT NULL AUTO_INCREMENT,
    `citizenid` varchar(50) NOT NULL,
    `level` int(11) DEFAULT 1,
    `xp` int(11) DEFAULT 0,
    `total_missions` int(11) DEFAULT 0,
    `total_earned` int(11) DEFAULT 0,
    `failed_missions` int(11) DEFAULT 0,
    `last_mission` timestamp NULL DEFAULT NULL,
    `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
    PRIMARY KEY (`id`),
    UNIQUE KEY `citizenid` (`citizenid`),
    KEY `level` (`level`),
    KEY `last_mission` (`last_mission`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

#### boosting_history Table
```sql
CREATE TABLE IF NOT EXISTS `boosting_history` (
    `id` int(11) NOT NULL AUTO_INCREMENT,
    `citizenid` varchar(50) NOT NULL,
    `vehicle_model` varchar(50) NOT NULL,
    `vehicle_name` varchar(100) NOT NULL,
    `mission_level` int(11) NOT NULL,
    `reward` int(11) DEFAULT 0,
    `xp_earned` int(11) DEFAULT 0,
    `status` enum('completed','failed','chopped') NOT NULL DEFAULT 'completed',
    `duration` int(11) DEFAULT 0,
    `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
    PRIMARY KEY (`id`),
    KEY `citizenid` (`citizenid`),
    KEY `status` (`status`),
    KEY `created_at` (`created_at`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

### Manual Database Reset (if needed)
```sql
-- Drop existing tables
DROP TABLE IF EXISTS `boosting_history`;
DROP TABLE IF EXISTS `boosting_players`;

-- Restart resource to recreate tables
```

---

## System Integration

### Discord Webhook Setup

1. **Configure in server.lua**
```lua
local DiscordWebhook = 'YOUR_WEBHOOK_URL_HERE'
local UseDiscordLogs = true

-- Configure which events to log
local DiscordLogs = {
    missionStart = true,
    missionComplete = true,
    missionFail = true,
    shopPurchase = true,
}
```

2. **Discord Webhook Format**
The script logs:
- Mission starts (player, vehicle, level)
- Mission completions (rewards, duration)
- Mission failures (reason, duration)
- Shop purchases (item, quantity, cost)

### Phone System Integration

Supported phones automatically receive mission briefings via email:

- **gksphone**: Native integration
- **qs-smartphone**: Native integration
- **qb-phone**: Native integration
- **yseries**: Custom mail handler included

**Email Content Includes**:
- Operative information
- Target vehicle details
- Reward breakdown
- Time limit
- Current reputation stats

---

## Usage Guide

### For Players

#### Getting Started
1. **Find the Boosting NPC**
   - Default location: Strawberry Avenue near LS Customs
   - Look for the smoking NPC with a target interaction

2. **Open the Menu**
   - Approach the NPC
   - Press the interaction key (default: E with ox_target)

3. **Select Difficulty Level**
   - Choose "Change Difficulty Level"
   - Select from your unlocked levels (you start at Level 1)
   - Higher levels = better vehicles, more rewards

4. **Accept a Contract**
   - Choose "Accept New Contract"
   - A random vehicle from your selected level will be assigned
   - Check your phone for contract details

#### Mission Workflow

**Step 1: Locate the Vehicle**
- GPS marker shows the area
- Navigate to the location
- Prepare for combat (NPCs guard the vehicle)

**Step 2: Eliminate Guards**
- Defeat all NPC guards protecting the vehicle
- Use cover and tactics
- NPCs have realistic combat AI

**Step 3: Enter the Vehicle**
- Vehicle unlocks when you approach
- **WARNING**: GPS tracker activates when you enter
- Police are immediately alerted

**Step 4: Disable the Tracker (Optional)**
- Use the hack item (Config.HackItem, default: plastic)
- Complete the hacking minigame
- **Success**: Tracker disabled, police lose signal
- **Failure**: Lose one attempt, vehicle takes damage
- Max attempts: 3 (configurable)

**Step 5: Deliver the Vehicle**
- Navigate to the delivery location
- **Random mode**: Meet the dealer NPC
- **Fixed mode**: Drive to marked zone
- Park near the dealer/zone

**Step 6: Complete Delivery**
- Interact with dealer NPC (if random mode)
- Vehicle condition is checked:
  - **Good condition**: Full rewards + XP
  - **Damaged**: Option to chop for reduced cash (no XP)
- Rewards are automatically given

#### Vehicle Damage System

**Damage Thresholds**:
- Engine Health: Must be above 300/1000
- Body Health: Must be above 500/1000

**If Damaged**:
- **Option 1**: Chop the vehicle
  - Receive 30% of reward (configurable)
  - No XP earned
  - Optional destruction animation
- **Option 2**: Abandon mission
  - Mission fails
  - No rewards

#### Boosting Shop

1. **Access the Shop**
   - Open main menu
   - Select "Boosting Shop"

2. **Browse Items**
   - View all available items
   - See prices in Nitro Cash
   - Check item descriptions

3. **Purchase Items**
   - Click on desired item
   - Enter quantity (1-100)
   - Confirm purchase
   - Items added to inventory

#### Statistics & Progression

**View Statistics**:
- Total missions completed
- Failed missions
- Success rate
- Total earnings
- Current level and XP
- Last mission timestamp

**Mission History**:
- View last 20 missions
- See outcomes (completed/failed/chopped)
- Check rewards earned
- Review mission duration

**Leaderboard**:
- Top 10 players globally
- Sorted by level, XP, and earnings
- See player names and stats

### For Police

#### Receiving Alerts

When a player enters a stolen vehicle:
1. **Initial Alert**
   - Sound notification
   - Screen notification: "VEHICLE THEFT IN PROGRESS"
   - GPS blip appears on map

2. **GPS Tracking**
   - Red blip shows vehicle location
   - Updates every 3 seconds (configurable)
   - Blip flashes to indicate active tracking

3. **Tracker Disabled**
   - If player successfully hacks, blip disappears
   - Notification: tracker disabled
   - Manual pursuit required

#### Responding to Calls

**Recommended Actions**:
- Coordinate with other officers
- Set up roadblocks
- Pursue the vehicle
- Attempt to stop/disable vehicle
- Arrest the suspect

**Notes**:
- Player has limited time (default 20 minutes)
- Vehicle may be damaged during pursuit
- Mission fails if player is killed/arrested

---

## API Reference

### Server-Side Events

#### Register a Mission Start
```lua
RegisterNetEvent('dj_boosting:server:startMission', function(targetLevel)
    -- Triggered when player accepts contract
    -- Parameters:
    --   targetLevel (number): Selected difficulty level
end)
```

#### Complete a Mission
```lua
RegisterNetEvent('dj_boosting:server:completeMission', function(missionData)
    -- Triggered on successful delivery
    -- Parameters:
    --   missionData (table): Mission information
end)
```

#### Chop a Vehicle
```lua
RegisterNetEvent('dj_boosting:server:chopVehicle', function(reward)
    -- Triggered when chopping damaged vehicle
    -- Parameters:
    --   reward (number): Calculated chop reward
end)
```

#### Mission Failed
```lua
RegisterNetEvent('dj_boosting:server:missionFailed', function()
    -- Triggered when mission fails
end)
```

#### Purchase Item
```lua
RegisterNetEvent('dj_boosting:server:purchaseItem', function(itemName, quantity, price)
    -- Triggered on shop purchase
    -- Parameters:
    --   itemName (string): Item identifier
    --   quantity (number): Amount to purchase
    --   price (number): Unit price
end)
```

#### Tracker Events
```lua
-- Tracker activated
RegisterNetEvent('dj_boosting:server:trackerActivated', function(coords)
    -- coords (vector3): Vehicle coordinates
end)

-- Tracker update
RegisterNetEvent('dj_boosting:server:updateTracker', function(coords)
    -- coords (vector3): Updated vehicle coordinates
end)

-- Mission ended (cleanup trackers)
RegisterNetEvent('dj_boosting:server:missionEnded', function()
    -- Removes all police trackers
end)
```

#### Hack Events
```lua
-- Hack success
RegisterNetEvent('dj_boosting:server:hackSuccess', function()
    -- Disables tracker, removes hack item
end)

-- Hack failed
RegisterNetEvent('dj_boosting:server:hackFailed', function()
    -- Decrements attempts, removes hack item
end)
```

### Server-Side Callbacks

#### Get Player Level
```lua
QBCore.Functions.CreateCallback('dj_boosting:server:getPlayerLevel', function(source, cb)
    local level, xp = GetPlayerLevel(source)
    cb(level)
end)
```

#### Get Nitro Cash
```lua
QBCore.Functions.CreateCallback('dj_boosting:server:getNitroCash', function(source, cb)
    local amount = GetItemCount(source, Config.CurrencyItem)
    cb(amount)
end)
```

#### Get Player Stats
```lua
lib.callback.register('dj_boosting:getPlayerStats', function(source)
    -- Returns: table with player statistics
    return {
        level = number,
        xp = number,
        total_missions = number,
        total_earned = number,
        failed_missions = number,
        last_mission = timestamp
    }
end)
```

#### Get Player History
```lua
lib.callback.register('dj_boosting:getPlayerHistory', function(source)
    -- Returns: array of last 20 missions
    return {
        {
            vehicle_model = string,
            vehicle_name = string,
            mission_level = number,
            reward = number,
            xp_earned = number,
            status = 'completed'|'failed'|'chopped',
            duration = number,
            created_at = timestamp
        }
    }
end)
```

#### Get Leaderboard
```lua
lib.callback.register('dj_boosting:getLeaderboard', function(source)
    -- Returns: array of top 10 players
    return {
        {
            name = string,
            level = number,
            xp = number,
            total_missions = number,
            total_earned = number,
            failed_missions = number
        }
    }
end)
```

### Client-Side Events

#### Start Mission
```lua
RegisterNetEvent('dj_boosting:client:startMission', function(missionData)
    -- Receives mission data and initializes mission
    -- missionData structure:
    {
        playerName = string,
        level = number,
        xp = number,
        targetLevel = number,
        vehicle = {
            model = string,
            name = string,
            reward = number,
            xp = number
        },
        startTime = timestamp
    }
end)
```

#### Police Alerts
```lua
-- Initial alert
RegisterNetEvent('dj_boosting:client:policeAlert', function(coords)
    -- coords (vector3): Vehicle location
end)

-- Tracker update
RegisterNetEvent('dj_boosting:client:updateTracker', function(coords)
    -- coords (vector3): Updated location
end)

-- Remove tracker
RegisterNetEvent('dj_boosting:client:removeTracker', function()
    -- Removes GPS blip
end)
```

#### Hack Events
```lua
-- Use hack item
RegisterNetEvent('dj_boosting:client:useHack', function()
    -- Initiates hacking minigame
end)

-- Hack success
RegisterNetEvent('dj_boosting:client:hackSuccess', function()
    -- Disables tracker, updates UI
end)

-- Hack failed
RegisterNetEvent('dj_boosting:client:hackFailed', function(attemptsLeft)
    -- attemptsLeft (number): Remaining attempts
end)
```

#### Menu Events
```lua
-- Open main menu
RegisterNetEvent('dj_boosting:client:openMenu', function()
    -- Displays main boosting menu
end)

-- Change level menu
RegisterNetEvent('dj_boosting:client:changeLevelMenu', function()
    -- Displays level selection
end)

-- Open shop
RegisterNetEvent('dj_boosting:client:openShop', function()
    -- Displays item shop
end)

-- View history
RegisterNetEvent('dj_boosting:client:viewHistory', function()
    -- Displays mission history
end)

-- View leaderboard
RegisterNetEvent('dj_boosting:client:viewLeaderboard', function()
    -- Displays top players
end)

-- Purchase item
RegisterNetEvent('dj_boosting:client:purchaseItem', function(item)
    -- Opens quantity selector for item
end)

-- Deliver vehicle
RegisterNetEvent('dj_boosting:client:deliverVehicle', function()
    -- Initiates delivery process
end)
```

### Server-Side Functions

#### Inventory Functions
```lua
-- Add item to player
AddItem(source, item, amount, metadata)
-- Returns: boolean (success)

-- Remove item from player
RemoveItem(source, item, amount)
-- Returns: boolean (success)

-- Get item count
GetItemCount(source, item)
-- Returns: number (count)
```

#### Player Level Functions
```lua
-- Get player level and XP
GetPlayerLevel(source)
-- Returns: level (number), xp (number)

-- Update player level
UpdatePlayerLevel(source, level, xp)
-- Returns: boolean (success)

-- Add XP to player
AddPlayerXP(source, xpAmount)
-- Returns: newLevel (number), newXP (number)
-- Auto-levels up player if XP threshold reached
```

#### Mission Management
```lua
-- Cleanup mission
CleanupMission(source, success)
-- Parameters:
--   source (number): Player source
--   success (boolean): Whether mission succeeded
-- Cleans up NPCs, vehicles, blips, and records history
```

#### Discord Logging
```lua
SendDiscordLog(title, description, color)
-- Parameters:
--   title (string): Embed title
--   description (string): Embed description
--   color (number): Embed color code
```

### Client-Side Functions

#### Notification
```lua
ShowNotification(msg, type)
-- Parameters:
--   msg (string): Message to display
--   type (string): 'info'|'success'|'error'|'warning'
```

#### Progress Bar
```lua
ShowProgress(label, duration, animDict, animName, flags)
-- Parameters:
--   label (string): Progress text
--   duration (number): Duration in ms
--   animDict (string): Animation dictionary
--   animName (string): Animation name
--   flags (number): Animation flags
-- Returns: boolean (completed)
```

#### Item Check
```lua
HasItem(item, amount)
-- Parameters:
--   item (string): Item name
--   amount (number): Required amount (optional, default 1)
-- Returns: boolean
```

#### Vehicle Fuel
```lua
SetVehicleFuelLevel(vehicle, level)
-- Parameters:
--   vehicle (entity): Vehicle entity
--   level (number): Fuel level (0-100)
```

#### Mission Functions
```lua
-- Spawn mission vehicle
SpawnMissionVehicle(data)

-- Spawn guard NPCs
SpawnGuardNPCs(location)

-- Spawn delivery dealer
SpawnDeliveryDealer()

-- Chop damaged vehicle
ChopDamagedVehicle(reward)

-- Dealer takes vehicle
DealerTakesVehicle()

-- End mission
EndMission(success)
```

---

## Troubleshooting

### Common Issues

#### 1. Script Won't Start
**Symptoms**: Script doesn't appear in resources list
**Solutions**:
- Check `fxmanifest.lua` exists
- Verify folder name matches `ensure` command
- Check for syntax errors in config
- Review server console for errors

#### 2. Database Tables Not Creating
**Symptoms**: Database errors in console
**Solutions**:
- Ensure oxmysql or mysql-async is running
- Check database connection in server.cfg
- Manually execute SQL from documentation
- Verify database user permissions

#### 3. Items Not Working
**Symptoms**: Can't use hack item, currency not found
**Solutions**:
- Add items to `qb-core/shared/items.lua`
- Add item images to inventory resource
- Verify `Config.HackItem` and `Config.CurrencyItem` match item names
- Restart qb-core and inventory resource

#### 4. NPC Not Spawning
**Symptoms**: Boosting NPC doesn't appear
**Solutions**:
- Check `Config.BoostingPed` coordinates
- Verify model name is valid
- Ensure target system is running
- Try different scenario or model

#### 5. Target Interaction Not Working
**Symptoms**: Can't interact with NPC
**Solutions**:
- Verify target system is detected (check console)
- Force target system in config if auto-detect fails
- Check target system is updated
- Increase interaction distance

#### 6. Phone Email Not Received
**Symptoms**: No mission details in phone
**Solutions**:
- Verify phone system is running
- Check `Config.PhoneSystem` matches your phone
- Ensure phone resource is updated
- Check phone integration code for your system

#### 7. Police Not Receiving Alerts
**Symptoms**: No tracker blip for police
**Solutions**:
- Verify job names in `Config.PoliceJobs`
- Check police are on-duty
- Ensure event names match
- Test with `/boostdebug` command

#### 8. Vehicle Damage Not Checking
**Symptoms**: Can deliver heavily damaged vehicles
**Solutions**:
- Set `Config.CheckVehicleDamage = true`
- Adjust `Config.MinEngineHealth` and `Config.MinBodyHealth`
- Check vehicle health in debug mode
- Verify damage checking code is not commented out

#### 9. Hacking Minigame Not Working
**Symptoms**: No minigame appears
**Solutions**:
- If using ox_lib: Ensure ox_lib is updated
- If using thermite: Ensure thermite resource exists
- Check `Config.UseOxLibSkillcheck` setting
- Verify minigame resource is running

#### 10. Leaderboard Shows Wrong Names
**Symptoms**: "Unknown Player" in leaderboard
**Solutions**:
- Check QBCore players table structure
- Verify charinfo field exists
- Update player names manually in database
- Check database connection

### Debug Commands

#### Server-Side Debug
```lua
-- Run in server console (requires admin permission in-game)
boostdebug

-- Shows:
-- - Active missions
-- - Global cooldown status
-- - Player cooldowns
-- - Player level/XP
-- - Nitro Cash balance
```

#### Client-Side Debug
```lua
-- Run in F8 console
boostdebug

-- Shows:
-- - Mission active status
-- - Tracker status
-- - Current vehicle
-- - Mission timer
-- - Selected level
-- - Detected systems
-- - Vehicle health (if in vehicle)
```

### Config Validation

The script automatically validates your configuration on startup:

**Errors** (Red):
- Critical issues that prevent proper function
- Must be fixed for script to work

**Warnings** (Yellow):
- Non-critical issues
- Script will work but may not be optimal

**Success** (Green):
- Configuration is valid
- No issues detected

### Log Files

Check these logs for errors:

1. **Server Console**: Real-time errors and warnings
2. **FiveM Server Log**: Full log history
3. **Database Logs**: SQL errors and queries
4. **Discord Webhook**: Mission activity logs (if enabled)

---

## Support & Credits

### Support

**Before Asking for Help**:
1. Check this documentation thoroughly
2. Review the troubleshooting section
3. Run `/boostdebug` command
4. Check console for errors
5. Verify all dependencies are installed

**Getting Support**:
- Create detailed bug reports
- Include error messages from console
- Provide config settings (redact sensitive info)
- Describe steps to reproduce issue
- Include FiveM version and framework version

### Credits

**Script Developer**: DJ Scripts
**Framework**: QBCore Framework
**UI Library**: ox_lib
**Database**: oxmysql

### License

This script is provided for use on FiveM servers. 

**Allowed**:
- Modify for personal use
- Use on multiple servers you own
- Integrate with other scripts

**Not Allowed**:
- Resell or redistribute
- Claim as your own work
- Remove or modify credits

### Version History

**v1.0.0** - Initial Release
- Core boosting system
- 15 reputation levels
- 90+ vehicles
- Police tracking
- Damage system
- Shop functionality
- Leaderboard system
- Multi-system compatibility

---

## Additional Resources

### Recommended Additions

**Minigame Resources** (if not using ox_lib):
- `thermite` - Hacking minigame
- `hacking` - Alternative hacking system
- `memorygame` - Memory-based minigame

**Enhancement Resources**:
- `ps-dispatch` - Advanced police dispatch
- `cd_dispatch` - Alternative dispatch system
- `t-notify` - Custom notifications

### Configuration Examples

#### High Difficulty Server
```lua
Config.MissionTimer = 15              -- 15 minute time limit
Config.HackAttempts = 2               -- Only 2 hack attempts
Config.NPCCount = 8                   -- More guards
Config.NPCHealth = 250                -- Higher health
Config.NPCAccuracy = 50               -- Better aim
Config.CheckVehicleDamage = true      -- Strict damage check
Config.MinEngineHealth = 500          -- Higher requirement
```

#### Casual Server
```lua
Config.MissionTimer = 30              -- 30 minute time limit
Config.HackAttempts = 5               -- More attempts
Config.NPCCount = 3                   -- Fewer guards
Config.NPCHealth = 100                -- Lower health
Config.NPCAccuracy = 15               -- Poor aim
Config.AllowChopDamaged = true        -- Allow chopping
Config.ChopRewardPercent = 50         -- Higher chop reward
```

#### RP-Focused Server
```lua
Config.UseRandomDelivery = true       -- Immersive delivery
Config.CheckVehicleDamage = true      -- Realistic damage
Config.ChopAnimation = true           -- Show chopping
Config.TrackerUpdateInterval = 5000   -- Slower updates
-- More police jobs for better RP
Config.PoliceJobs = {'police', 'sheriff', 'state', 'bcso', 'sasp'}
```

### Performance Optimization

**For High-Population Servers**:
```lua
-- Reduce update intervals
Config.TrackerUpdateInterval = 5000   -- 5 seconds instead of 3

-- Limit NPC count
Config.NPCCount = 4                   -- Instead of 6-8

-- Disable heavy animations
Config.ChopAnimation = false          -- Skip destruction animation
```

**For Low-End Servers**:
```lua
-- Disable optional features
Config.UseOxLibNotifications = false  -- Use QBCore notifications
Config.UseOxLibProgress = false       -- Use QBCore progress
```

---

## Conclusion

Thank you for using **DJ Vehicle Boosting System**! This comprehensive script provides an engaging criminal activity for your FiveM server with deep progression, realistic mechanics, and extensive customization options.

For updates, support, and more scripts, stay tuned to DJ Scripts.

**Happy Boosting!** 🚗💨

---

*Documentation Version: 1.0.0*  
*Last Updated: 2026*  
*Compatible with: QBCore Framework (Latest)*
