A week later than planned, but here is the result of the paper-to-fabric transition test that Nether and I performed last weekend, but which I didn't have the time to write down yet. The results will be split over multiple messages due to the Discord message length limit. There is a very brief summary at the end of this message.

### Terms used:
* online mode / offline mode - Whether the `online-mode` property in the server.properties is `true` or `false` respectively. In offline mode, people with cracked accounts can join too, but no authentication is performed.
* official java / bedrock - A player who has legitimately bought the game, and thus does not rely on a cracked version.

### Test setup
For the test, I set up my server with both a paper, and a fabric setup. The paper plugins were the same as the current plugins on the chill server, except for the "wolfie-script" specific plugins. The fabric mods used are listed further down. The steps in the testing are listed directly below. These steps were chosen as these best represent the phases of by the official Chill Penguin server (offline mode 1.19.1 on aternos, online mode 1.21.1 directly after the transition, offline mode 1.21.1 more recently again)

1. Initially, the server was started in offline mode. During this phase, we joined with both official + cracked java, as well as with official bedrock. Cracked bedrock was not tested since neither of us has access to that, and when Safe did use it, his experience was already horribly ad-infested making him effectively unable to join at all anyway.
2. Next, the server was restarted, this time in online mode. In this phase, we logged in with official java and bedrock.
3. Thirdly, the server was put back into offline mode, once again logging in with both official + cracked java, and official bedrock.
4. Lastly, the server was transitioned to fabric in online mode, but with an authentication mod to allow cracked players to join too. Here, we logged in with both official and cracked java + official bedrock.

### Results
Transitioning from paper to fabric did not cause any issues. The performance was good on both versions, and no weird behaviour was observed with the world itself. There are some side notes regarding online mode vs offline mode that deserve mentioning:
When toggling the server between online and offline mode, it is possible that players' statistics (blocks broken, items used, unlocked recipes, etc.) as well as player inventories don't follow the most up-to-date data. This behaviour is also what caused the initial disappearance of the special items. When the server transitions from online to offline or the other way around, the server can have difficulties with identifying which offline mode java account corresponds to which online mode java account. As such when the player has joined both offline and online, the player effectively has two inventories: Their offline mode inventory, and their online mode inventory. Which inventory + statistics they see, depends on the mode (online / offline) the server is in, and whether they have previously joined in that specific mode.
Assuming the server is set to online mode (as I'm proposing with fabric): If they never joined online before, the server will take their last known offline inventory, and copy that to their online inventory. If they have joined online before, they will see their last saved online inventory. This will therefore likely only really affect me, Nether, Kaz, and Boop (I'm not aware of anyone else who was notably online when the server was in online mode just after the transition), but can be worked around by having these players dump their important items in a chest before the transition to online mode / fabric. Even so, resolving this is rather trivial assuming a player with Creative mode access is there to quickly help people restore their lost items.

### Transition steps
The transition from paper to fabric itself is rather simple:
1. Download the most recent fabric server jar, either from https://fabricmc.net/use/server/ or using the server management dashboard if the server hosting platform has that.
2. Download the mods below, and put them in the `/mods/` folder. If it does not yet exist, this mods folder should be alongside the world/ folder, and the server.jar file.
3. Move the following files:
    1. `/world_nether/DIM-1` to `/world/DIM-1`
    2. `/world_the_end/DIM1` to `/world/DIM1`
    3. Note: If a `DIM-1` folder, or a `DIM1` folder already exist inside the `world` folder, replace those with the ones from `world_nether/` and `world_the_end/`.
4. Set `online-mode` to `true` in the `server.properties` file.
5. Verify that `enforce-secure-profile` is set to `false` in the `server.properties` file.
6. [Optional] Put the datapack(s) listed below inside the `/world/datapacks/` folder. This is only needed in case the custom textures need to be fixed.
Once this is done, the server should be ready to run fabric.

### Fabric mods:
The information in <> denotes the mod version used in testing.
1. [GeyserMC](https://geysermc.org/download)
2. [Floodgate](https://modrinth.com/mod/floodgate) <2.2.4-b36>
3. [Fabric API](https://modrinth.com/mod/fabric-api) <0.105.0+1.21.1>
4. [EasyAuth](https://modrinth.com/mod/easyauth) <3.0.25 - 1.21.x>
5. [Lithium](https://modrinth.com/mod/lithium/versions) <mc1.21.1-0.13.1>
6. [Ferrite Core](https://modrinth.com/mod/ferrite-core) <7.0.0>
7. [Convenient MobGriefing](https://modrinth.com/mod/convenient-mobgriefing) <2.1.2>
7. [worldedit](https://modrinth.com/plugin/worldedit) <7.3.6 fabric>
8. [ViaVersion](https://modrinth.com/plugin/viaversion) <5.0.3>
9. [ViaFabric](https://modrinth.com/mod/viafabric) <0.4.15+84-main (Alpha release)> - Allows ViaVersion to run on Fabric

Notes:
* ViaVersion and ViaFabric are currently not needed yet. Since 1.21.1 is already the most recent version, they do not add any new compatible versions. Once 1.21.2 comes out, they will need to be updated regardless.
* Lithium and Ferrite Core are both performance mods. They do not add any new features, but significantly improve performance to roughly the same level as Paper.
* Logging in:
    * If a player with an official account logs in, they will be able to join instantly, with them receiving the chat message: `You are using an online account. No need to log in.`
    * If a player with cracked MC joins, the first time they will be asked to use `/register <password> <password>` to set up a password for that user. The next times they log in, they must type `/l(ogin) <password>` in chat to join again with that account. They will also be prompted in chat to do this. This prevents anyone from logging in as another user. Note: Their username should not yet be the username of an existing official Minecraft account. If it is, their username will need to be set as "forced offline" using `/auth addToForcedOffline <player>` by an admin.

### Datapacks
1. [Custom Roleplay Data](https://www.curseforge.com/minecraft/customization/custom-roleplay-data-datapack)

### Other
I would like to see if I can combine the statistics of the offline mode phase and the online mode phase into one, to get complete statistics again (having the correct number of blocks mined again). For this, I would need the data from the `/world/playerdata/` folder, and the `/world/stats/` folder. Do you think it would be possible to send these two folders so I can attempt to combine these statistics?

### Summary
There should be no significant obstacles in converting the server from paper to fabric. Simply follow the steps in `Transition Steps`, and everything should configure itself correctly.
The advantages of a transition to fabric are:
1. Improved server security (can no longer log in as another player)
2. Improved compatibility with Vanilla behaviour / farms

If you have any other questions / concerns, feel free to ask.
