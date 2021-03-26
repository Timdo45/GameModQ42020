DROP LOOT FEATURE
	CHANGES IN AI.CPP
	
		//IT266 Modification to existing unused command in onDeath() method to drop items.
		Previous code was the rVal variable and an if statement that would drop "item_health_small"
	
		float rVal = gameLocal.random.RandomInt( 100 );

		if (rVal < 10){
			spawnArgs.Set("def_dropsItem1", "ammo_machinegun");
			spawnArgs.Set("def_dropsItem5", "ammo_nailgun");
			spawnArgs.Set("def_dropsItem6", "ammo_lightninggun");
			spawnArgs.Set("def_dropsItem7", "ammo_shotgun");
			spawnArgs.Set("def_dropsItem8", "ammo_hyperblaster");
			spawnArgs.Set("def_dropsItem9", "ammo_railgun");
			spawnArgs.Set("def_dropsItem10", "ammo_napalmgun");
			spawnArgs.Set("def_dropsItem11", "ammo_grenadelauncher");
			spawnArgs.Set("def_dropsItem12", "ammo_rocketlauncher");
			spawnArgs.Set("def_dropsItem13", "ammo_dmg");
		}
		else if (rVal < 50){
			spawnArgs.Set("def_dropsItem1", "ammo_machinegun");
			spawnArgs.Set("def_dropsItem7", "ammo_shotgun");
		}
		else if( rVal < 75 ){
			spawnArgs.Set("def_dropsItem2", "item_health_small");
			spawnArgs.Set("def_dropsItem3", "item_armor_small");
		}
		else if (rVal < 100){
			spawnArgs.Set("def_dropsItem2", "item_health_small");
		}
	}
		//IT266 different elseif statements that have enemies drop different items depending on a random int variable.
		
RANDOMIZED WEAPON STATS
	
	NEW VARIABLES DECLARED IN WEAPON.H
		
		float fireRate_Random
		float spread_Random
		int clip_Random
	CHANGES IN WEAPON.CPP
		
		clipSize= spawnArgs.GetInt( "clipSize" ) + clip_Random; //IT266 added random additional clip size to weapon clip
		//IT266 random variables defined
		fireRate_Random = gameLocal.random.RandomFloat();
		if (fireRate_Random >= 0.5){
			fireRate_Random == 0.5f;//IT266 Firerate felt too bad when above this rate so this 0.5 is the cap
		}
		clip_Random = gameLocal.random.RandomInt(50);
		spread_Random = gameLocal.random.RandomInt(5);
		int randomadd = gameLocal.random.RandomInt(1);//IT266 variable to switch between adding or decreasing spread
		if (randomadd == 0){
			spread_Random -= 2 * spread_Random;
		}

		fireRate = SEC2MS(  fireRate_Random); //IT266 replaced firerate with random number
		altFireRate	= SEC2MS ( spawnArgs.GetFloat ( "altFireRate" ) - fireRate_Random );//IT266 subtratcting altFireRate with the random firerate variable
		
		//IT266 added the random spread variable to the initial calclulation for spread
		spread		= (gameLocal.IsMultiplayer()&&spawnArgs.FindKey("spread_mp"))?spawnArgs.GetFloat ( "spread_mp" ):spawnArgs.GetFloat ( "spread" ) + spread_Random;
		
		//IT266 all of these were written in the Spawn() method so these change whenever the player switches back to a gun from a previous one
	NEW VARIABLES DECLARED IN ENTITY.H
		
		//IT266 Modification damage randomizer
		int damage_Random = gameLocal.random.RandomInt(100);
	CHANGES IN ENTITY.CPP
		
		//IT266 added the damage randomizer to the existing damage variable in the Damage() method
		int	damage = damageDef->GetInt("damage")+damage_Random;
		
		//IT266 this actually makes it so each shot does random additional rather than the gun itself. 
		I'm also pretty sure that theis makes it so all entities do more random damage.
GUI CHANGES
	
	CHANGES IN MAINMENU.GUI
		
		Edited the main menu using the ingame gui editor to make it obvious it was the modded version by adding "modded" above
		the Quake 4 Title. Also changed the color of the background from a dark green to a light blue.

	CHANGES IN PLAYER.CPP
		
		//IT266 modified weapon name display in the UpdateHudWeapon() method. Used to display name of weapon when equipped, but I switched it with just the string
		"modified weapon"
		hud->SetStateString( "weaponname", "modified weapon" );

