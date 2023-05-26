The game update logic that gets run whenever a player starts the game goes like this:

1. Check the previous_game_end_time and the game_start_time. Calculate time_elapsed in epoch second.

2. Game state is updated every 60 seconds. However, within each game state loop, any kind of counter is decremented or incremented by one, not seconds. To be more clear, within each game state loop, there is no concept of time. Everything is counted by "counter".

3. Each game state loop contains the change the game should undergo by the end of the loop. 

4. If a cat decides to stop playing, the cat will only leave the toy at the end of game state loop. Even if there is another cat wanting to play with the same toy, that new cat needs to wait until the next game state loop.

5. For each game state loop

    1. Get the list of room_items in the room.

    2. For each room_item in the room_items list
    
        1. If room_item is occupied by a cat

            1. Reduce the cat's remaining_attention_to_toy_counter by 1

            2. If the remaining_attention_to_toy_counter reaches 0
            
                1. Calculate rewards from this cat

                2. remove the cat from the room and from the toy

                3. Update the reward_counter of this cat on the rewards list.

        2. If room_item is not occupied

            1. Create a eligible_cats list. This list contains cats that are not in anyone's room.

            2. From the eligible_cats list, create a cats_matching_favorite_item list of cats whose favorite item is the room_item.

            3. Randomly select an attracted_cat from the cats_matching_favorite_item list.

            4. Add attracted_cat to the room. This includes updating the list of cats in the room and the cat's occupied item.

6. Update the rewards list after going through all the game state loop. This reward list includes all the cats that showed up during the time elapsed and the total number of rewards the cat gave.
