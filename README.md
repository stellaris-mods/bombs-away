# bombs-away
Bomb weapons, mostly as a proof of concept.

```
folk: it only supports one component of each type per ship, so it really should be a required_component_set instead I guess, but the point was also not to have to modify ship_sizes

folk: also, this particular method adds a bomb as a ship in your fleet, which is strictly not necessary, it was just the easiest for me to do

folk: the better thing would be to spawn it as a space object that is hostile to the target fleet

folk: also, you can do it without using the ship_behaviors, but it's important to note that the fleet queue_actions move_to command does not update to reflect the targets position, so you need to repeat={} it continously in order to track a target
```

The main reason we use a ship_event per bomb is that it allows us to set an independent event_target target per bomb. The current code doesn't do that, however.

One problem with spawning the bombs inside your own fleet, as your own ships, is that they require the same FTL type as the fleet.

Another problem is that you need to track combat status properly - I probably don't do that in the code here currently, so the bombs can probably survive until the next engagement. I haven't tested anything.

Obviously the bomb would need to create some explosion, but you can just create an ambient object at the bombs location when it reaches its target.

The bomb in the code only spawns once when you engage an enemy, and then never again. This is obviously easy to change with an event like
```
ship_event = {
	id = bombs.2
	hide_window = yes

	trigger = {
		NOT = {
			has_ship_flag = bombs_regen_timer
		}
		has_component = BOMB_THING
		is_in_combat = yes
	}

	mean_time_to_happen = {
		days = 30
	}

	immediate = {
		launch_another_bomb = yes
		set_timed_ship_flag = {
			flag = bombs_regen_timer
			days = XYZ
		}
	}
}
```
And then obviously set the same timed_ship_flag in the `on_entering_battle` handler.
