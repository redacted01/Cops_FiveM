# Cops_FiveM
[![Version](https://img.shields.io/badge/Version-v1.4.0-brightgreen.svg)](https://github.com/Kyominii/Cops_FiveM/releases/tag/v1.4.0)

## Features
Cops_FiveM is a script for RP server mainly. It let servers to have a cops system with loadout, vehicles, inventory check, ...    
You can find the complete list with all the features [here](docs/features.md)

## Changelog
You can find the changelog [here](https://github.com/Kyominii/Cops_FiveM/blob/master/CHANGELOG.md)

## Support
A Discord server is now available : [![](https://discordapp.com/api/guilds/361144123681538060/widget.png)](https://discord.gg/yBtN2bc)

## Installation
* Install supported scripts you want
* Download police folder from this [git](https://github.com/Kyominii/Cops_FiveM) and rename it police
* Put this folder to resources folder in your server
* Add [sql file](https://github.com/Kyominii/Cops_FiveM/blob/master/police.sql) in your database
* Edit [config file](https://github.com/Kyominii/Cops_FiveM/blob/master/police/config/config.lua) as you want
* Add "start police" in server.cfg (make sure you start this resource after all dependencies)

## Upgrade
The database has changed with the v1.4.0, so you have to execute [upgrade file](https://github.com/Kyominii/Cops_FiveM/blob/master/upgrade-1.3-to-1.4.sql) on your database to migrate to the new police database.


## Contribute
If you are a developer and  would like to contribute any help is welcome!   
The contribution guide can be found [here](https://github.com/Kyominii/Cops_FiveM/blob/master/CONTRIBUTING.md).

(Readme, Contributing and Changelog files from by [FiveM Script](https://github.com/FiveM-Scripts/), thanks ^^)

## Commands 
**You need to add a rank for each cop, configure the `minRankSetRank` in the config file.** 

* /copadd ID : to add a policeman in the database
* /coprem ID : to remove a policeman from the database
* /coprank ID Rank : To change the rank of a police officer

## Ranks
| ID | Name |
| -- | ---- |
| 0  | Trainee|
| 1  | Trooper|
| 2  | Master Police Officer|
| 3  | Sergeant|
| 4  | Lieutenant|
| 5  | Captain|
| 6  | Chief of Police|
| 7  | Admin Police Rank|

# Supported scripts
* [mysql-async](https://forum.fivem.net/t/beta-mysql-async-library-v0-2-2/21881)
* [fs_freemode](https://github.com/FiveM-Scripts/fs_freemode)
* [Vdk_inventory](https://forum.fivem.net/t/release-inventory-system-v1-4/14477)
* [Simple Banking](https://forum.fivem.net/t/release-simple-banking-2-0-now-with-gui/13896)

If you are using this script, add this piece of code in server.lua (banking)


```lua
RegisterServerEvent('bank:withdrawAmende')
AddEventHandler('bank:withdrawAmende', function(amount)
  TriggerEvent('es:getPlayerFromId', source, function(user)
      local rounded = round(tonumber(amount), 0)
      if(string.len(rounded) >= 9) then
        TriggerClientEvent('chatMessage', source, "", {0, 0, 200}, "^1Input too high^0")
        CancelEvent()
      else
        local player = user.identifier
        local bankbalance = bankBalance(player)
        withdraw(player, rounded)
        local new_balance = bankBalance(player)
        TriggerClientEvent("police:notify", source, "CHAR_BANK_MAZE", 1, "Maze Bank", false, "Withdrew: ~g~$".. rounded .." ~n~~s~New Balance: ~g~$" .. new_balance)
        TriggerClientEvent("banking:updateBalance", source, new_balance)
        TriggerClientEvent("banking:removeBalance", source, rounded)
        CancelEvent()
      end
  end)
end)
```

* [JobSystem](https://forum.fivem.net/t/release-jobs-system-v1-0-and-paycheck-v2-0/14054)
* [Skin Customization](https://forum.fivem.net/t/release-skin-customization-v1-0/16491)
* [Player in db](https://forum.fivem.net/t/release-nameofplayers-v-1-get-name-of-players-in-database/17983)
* [es_weashop](https://forum.fivem.net/t/release-es-weapon-store-v1-1/12195)
* [garages](https://forum.fivem.net/t/release-garages-v4-1-fr-en-03-06-17-updated/13066)
* [emergency](https://forum.fivem.net/t/release-job-save-people-be-a-hero-paramedic-emergency-coma-ko/19773)

If you are using this script, add this piece of code in cl_healthplayer.lua (line 196 -- function ResPlayer() -- emergency)

```lua
TriggerEvent("es_em:cl_ResPlayer")
```

* [Heli Script](https://forum.fivem.net/t/release-heli-script/24094) : it is not really "supported" because it's working without anything, but I recommand this script for the cop helicopter
* [gc_identity](https://github.com/Gannon001/gcidentity)

If you are using this script, add this event in server.lua (gc_identity)
```lua
RegisterServerEvent('gc:copOpenIdentity')
AddEventHandler('gc:copOpenIdentity',function(other)
    local data = getIdentity(other)
    if data ~= nil then
        TriggerClientEvent('gc:showItentity', source, {
            nom = data.nom,
            prenom = data.prenom,
            sexe = data.sexe,
            dateNaissance = tostring(data.dateNaissance),
            jobs = data.jobs,
            taille = data.taille
        })
    end
end)
```

## Thanks to the whole community of FiveM which help to improve this script
