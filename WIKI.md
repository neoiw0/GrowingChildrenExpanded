# Growing Children Expanded 2.9.43 — Player Wiki

## English table of contents

1. [Requirements and installation](#en-1-requirements-and-installation)
2. [Identity, parents, and multiplayer](#en-2-identity-parents-and-multiplayer)
3. [Growth, movement, and personality](#en-3-growth-movement-and-personality)
4. [Parent bond](#en-4-parent-bond)
5. [Work, tools, and produce quality](#en-5-work-tools-and-produce-quality)
6. [Appearance, hair minerals, and boots](#en-6-appearance-hair-minerals-and-boots)
7. [Home life, beds, hugs, and television](#en-7-home-life-beds-hugs-and-television)
8. [Conversation, serious talks, and emotions](#en-8-conversation-serious-talks-and-emotions)
9. [Careers, farewell, and letters](#en-9-careers-farewell-and-letters)
10. [Festivals, routes, and rain](#en-10-festivals-routes-and-rain)
11. [Health, breakfast, and daily care](#en-11-health-breakfast-and-daily-care)
12. [The warrior: mining and combat](#en-12-the-warrior-mining-and-combat)
13. [Settings page, shared time flow, and leisure mode](#en-13-settings-page-shared-time-flow-and-leisure-mode)
14. [Safe uninstall](#en-14-safe-uninstall)
15. [Testing boundary](#en-15-testing-boundary)

## Full English wiki

### EN 1. Requirements and installation

Growing Children Expanded requires Stardew Valley 1.6, SMAPI 4.0 or later, Growing Children 1.0.2, and SpaceCore 1.28.4 or later. Generic Mod Config Menu is optional.

Keep the original Growing Children mod installed. Install this expansion as a separate folder under `Mods`. When updating, replace the expansion files but preserve your existing `config.json` if you want to retain settings.

When GMCM is installed, the Expanded page also owns the four settings exposed by the original Growing Children mod: toddler duration, automatic growth, maximum children, and base debug logging. Existing base-mod values are imported once, then bridged to every local base-mod instance. Merely starting the game or deploying an update does not rewrite `config.json`; saving GMCM or explicitly changing safe-uninstall state persists the requested change.

The same GMCM page also configures the four native growth stages (crib, baby, crawler, and toddler days; default 7/7/7/7, from 3 days up to twice the vanilla length) and the bedtime sleep tips (an enable switch and a daily or occasional frequency).

### EN 2. Identity, parents, and multiplayer

Each child has a durable child ID. Names, temporary NPC instances, portraits, local split-screen views, and remote clients are treated as representations of that identity rather than independent children.

The host owns save mutations. A local split-screen farmhand delivers input to the host instance in the same process. A remote farmhand sends a directed request; the host validates that the sender is a recorded player parent, commits once, then broadcasts state and sends any parent-only notice to the correct player.

NPC spouses may be recorded as NPC parents. They can participate in suitable life interactions such as a child walking over for a hug. They never receive a player relationship ledger, mail, rewards, configuration prompts, bed questions, or planting-area questions.

Families also change over time. After a divorce the child keeps their history: for about one season (28 days) the child may show sad or confused dialogue and bond gains are temporarily halved. The former parent keeps their historical identity but no longer writes daily values. After a remarriage the new spouse starts from zero and does not inherit old relationship points.

### EN 3. Growth, movement, and personality

Visual growth is gradual: a newly grown child starts near 50% of the ordinary adult body size and smooths toward the adult and mature sizes over the configured growth years. Growth is tracked in ten visible stages, each reached when the body grows another 10%. Each stage is announced once, on the morning it is first reached, with wording that matches the child's personality; reloading the save does not repeat it.

Native babies follow their own curve: during each native stage (crib, baby, crawler, toddler) the child grows from 50% toward 70% of the base sprite size and reaches exactly 70% on the grow-up day, with the timeline equal to the sum of the four configured stage lengths. The adult transition then runs from 50% to 100%; mature boys reach 110% in both dimensions, while mature girls reach 105% height with 100% width. Personality body factors (Spirited 1.08, Independent 1.04, Easygoing 1.02, Curious 0.98, Thoughtful 0.96, Careful 0.92) apply at every stage.

Six personalities (Spirited, Independent, Curious, Easygoing, Thoughtful, Careful) affect body scale, walking, work pauses, dialogue weighting, and how strongly individual bond sources feel. The source multipliers rotate across personalities so one personality's advantage in one activity is balanced by smaller gains elsewhere.

The movement base is 100% of the player's unbuffed normal running speed. The child then applies the existing growth, personality, and parent-bond factors. While a child is actually crossing maps for work, returning home, or travelling to an evening activity, the base temporarily becomes 120%; it returns to the normal work or leisure base as soon as the destination map is reached. A warrior travelling all the way to or from the mine uses 150% for that trip only. Indoor evening leisure and reached resting places use a 60% base, and Saloon leisure uses 40%. During a Skull Cavern rescue chase, speed rises continuously with distance up to a safe cap of 250%.

`bond speed factor = 0.80 + 0.25 × t`

where `t = average real-player-parent bond / 2500`, clamped to `0..1`. Minimum bond therefore gives `-20%`; maximum bond gives `+5%`. NPC parents are excluded from the average because they have no player ledger.

Movement ownership is attached to the authoritative child representation and its route controller. It is not repaired through periodic position or speed mirroring.

### EN 4. Parent bond

The scale is `0..2500` per real-player parent. One heart is 250 points and half a heart is 125. Notices use warm, everyday wording and show the exact numeric result in parentheses, even when the applied value is zero (for example a missed breakfast still shows the requested `-200`). At the full bond the notice shows `(∞)` with a warm line instead of a plain zero, because the moment still matters.

Bond sources:

| Event | Base change | Limit |
|---|---:|---|
| Ordinary daily conversation | +50 | Once per child and player parent per day |
| A toy is loved until it breaks | +250 | When the durable toy transaction completes; credited to its giver |
| Sleeping on the floor | -125 | Once per day for each real-player parent |
| Seasonal clothing or hair change | +125 | Once per season per child and acting parent |
| Sleeping in a proper bed | +10 | Once per day for each real-player parent |
| Missing-parent moment after completed work | +20 | Deterministic 20% opportunity; awarded whether physical hug contact succeeds or not |
| Surprise gift | +250 | When the surprise-gift transaction commits |
| Breakfast | -200 / -25 / +25 | Daily; see the health section |

Personality rotates balanced multipliers across these sources. Negative events are also recorded as individual loss transactions, which lets an apology recover only real recent losses instead of inventing points.

Apology (part of "Have a heart-to-heart talk") restores the unrecovered portion of real losses from the current or previous two days. Each loss record keeps its own two-day window; only a successful apology marks it as recovered. When no candidate loss exists, no apology result is shown.

### EN 5. Work, tools, and produce quality

Children retain the original farm professions: farmer, animal caretaker, fisher, forager, and warrior (internally still `Mineiro`). Tool actions use the committed child appearance, tool tier, facing direction, target tile, swing frames, and pause timeline as one presentation transaction.

Farmers discover and commit tilling, watering, planting, fertilizer, and harvest as distinct native farm actions. Fertilizing is its own farm target and is found before the farmer finishes the day's farm work. If the seed supply runs out, the farmer trims the remaining planting plan and immediately moves on to watering instead of walking to empty plots and miming empty sow actions. Foragers plan at most 25 reachable targets, revalidate each weed, twig, stone, forage object, or artifact spot immediately before the host deposits its result and removes the exact source. A scythe swings from the tile the child already stands on and cuts the neighbouring tile, so the child never steps into the grass it is cutting. When the farm is fully gathered, the forager may continue to one random legal non-farm map. Fishers save a parent-selected place (or a new legal random place for each trip), cast with a real backpack rod, wait 10-30 game minutes with a visible line and bobber, then reel in one exact water-table result. Higher rod tiers shorten the maximum wait. A catch only counts after the backpack accepts it.

Produce quality starts from the growth stage and profession level, gains at most a 25% upgrade chance from the experience inside the current level, and then applies the active emotion step (`+1` / `-1` tier) clamped to the native ladder `normal -> silver -> gold -> iridium`. Silver unlocks from work day 8, gold from work day 15. Iridium starts from day 21 at maximum bond and day 112 (one in-game year) at minimum bond:

`iridium unlock day = ceil(112 - 91 × t)`

Only the committed bond value at production time is used. A previously created item is never retroactively upgraded. Food eaten for breakfast can temporarily raise the profession level for the rest of that day (farm and ranch use Farming, fisher uses Fishing, forager uses Foraging, warrior uses Mining, at most to level 10).

Each child has one mutually exclusive work-event roll per working day, at a random time after 12:00: 2% a small work accident, 2% a found treasure, 96% nothing. Every profession has five accident and five treasure texts. When work ends and the child enters the farmhouse, there is a 50% chance of an "I'm home!" bubble visible only to parents within five tiles. If the backpack is full, the child actively reports instead of silently losing items.

Bond modifies pauses between tool actions:

`bond pause factor = 1.50 - 0.80 × t`

Minimum bond gives `+50%` pause; maximum bond gives `-30%`. Personality is multiplied with this factor.

#### Storage returns and animal feedback

"Put things away" is a host-owned transaction. The child visits supported vanilla chests, big chests, and fridges, but only adds to an existing compatible stack which is not full; empty slots never create a new stack. Different items may therefore lead to different containers. A successful physical container visit produces one lid animation plus one parent HUD notice listing the items actually deposited. Failed, unmatched, or capacity-blocked stacks remain in the backpack and are never reported as successful.

All real `Tool` items, including watering cans, milk pails, shears, fishing rods, and weapons, remain in the child's backpack. Active treasured toys are protected too. Animal care is committed once by the host; observing screens only present the pet sound/emote or the pass-through jump. Larger animals use a lower jump than smaller animals without changing their world position or AI.
### EN 6. Appearance, hair minerals, and boots

Hair and clothing changes are committed to the child identity, then used by world sprites, menus, portraits, work animations, warps, and split-screen views. Hat, shirt, and pants are part of the same committed appearance. Tool animation does not rebuild from an older hair state.

Recognized hair minerals include normal gems and diamond. Diamond produces a near-white color with a slight blue tint: RGB `242, 248, 255`. A Prismatic Shard is a special gift: when a parent puts one into the backpack and closes it, the child picks a new hairstyle for their gender and a random hair color, then answers with one of ten reaction lines.

The backpack's first durable-ledger migration recognizes existing hair minerals and toys rather than relabeling every item as a generic system reward. Only a real player personally adding a qualifying artifact in the current backpack session makes it a treasured toy; artifacts gathered by work are always work results, never toy sources.

Boots are a fourth wear slot alongside hat, shirt, and pants. Hold any pair of boots and interact with a child to put them on: the held pair is consumed, and if the child already wore boots the old pair returns to your inventory. The child menu's clothes page equips or removes all four slots (equipping consumes the item from your inventory; removing returns it). The child's world and work sprites are coloured by the worn boots; legacy saves keep their old shoe colour as a fallback while no boots are worn. When a parent puts a hat, shirt, pants, or boots into the child's backpack and closes it, the child automatically wears it if that slot is empty: the item is consumed and recorded in the backpack ledger, the child replies with a warm line, and the quarterly +125 appearance bond applies to the giver. Items placed by work never auto-trigger this.

### EN 7. Home life, beds, hugs, and television

An adult child resolves one recorded residence and bed transaction. A proper bed gives the daily +10 bond event. If no usable bed exists, the child sleeps on the floor and loses half a heart. Floor bedding uses a clear 3x2 footprint nearest to a recorded parent when possible, with a visible pillow and blanket derived from the default bed material and pale earthy-yellow stripes. The whole bedding stack stays below players and overlapping furniture.

Every sleeping child receives a visual posture update every ten in-game minutes: 80% side sleeping, 15% flat on their back, and 5% face-down with the back of the head visible. Only the child's front, side, or back view changes; the bed, pillow, and blanket do not rotate.

Vanilla toddlers keep their original route to `GetChildBedSpot`, destination callback, pre-sleep random toy animation, age-based drawing scale, and native bed anchor. After native sleep is committed, Expanded only blocks later competing animation writes while the child remains asleep. Waking immediately returns animation ownership to the game.

Television is a daily routine. Morning TV starts its five in-game-minute viewing period only after the screen is actually on and reports the real parent's Daily Luck with the game's own current-language fortune terms. Evening TV uses thirty in-game minutes and reports tomorrow's weather. During daytime home leisure a child may watch a living, cooking, or fishing programme instead of repeating the morning fortune or evening weather. For each farmhouse and time period, the first child who successfully reserves a television watches; other children skip that period without queueing. If all real parents are away, the child may leave the set on as ambient sound; that screen does not reserve the TV and a later viewer simply switches the programme.

Hugs come from the existing probability and cooldown gates. A child first chooses the nearest legal real parent on the same map, and may approach another online parent through the public travel system when none is near. Player birthdays and stormy nights before bed always grant a hug. A held-overhead item is never cloned, removed, or reinserted; the slot, item reference, facing, and carrying presentation are restored after the hug. Children may also approach a recorded NPC parent for a visual hug that creates no bond, mail, or reward state. When two grown children meet within two tiles and both are off cooldown, the host creates one meeting: they stop, face each other, and chat briefly before the original owners resume. If one is working or travelling, 80% of these meetings use short busy lines.

**Sleep tips.** At bedtime the mod shows one small tip during the day-rollover black window: it stays visible while "Saving..."/"Progress saved" is on screen and on the settlement main page, and fades out when the transition ends; opening the detailed settlement list hides it immediately. In single player the tip sits at the bottom centre, with its lowest point aligned to the vanilla saving text and its middle character aligned to the screen centre; in local split-screen each screen centres it on the shared window seam. The host picks one tip per night (avoiding the previously shown one), and every screen shows the same tip. Up to eight extra tips can queue and play one per night. The 46-tip bilingual pool covers hidden GCE mechanics; the feature and its frequency (daily or roughly every other night) are configurable.

### EN 8. Conversation, serious talks, and emotions

The child menu keeps all original options. "Have a heart-to-heart talk" appears immediately before "Maybe later". It contains:

- Apologize
- Praise them
- Correct them
- Encourage them
- Listen to their complaint
- Share something difficult
- Ask about their learning progress
- Ask a mature child to stay a little longer, when available
- Back

Apology has 20 authored, personality-tagged warm replies. Once per child and player parent per day, the host makes a deterministic 50% decision. On success, it restores the unrecovered portion of real losses recorded during the current or previous two days, as described under the bond section.

Every serious-talk option has a pool of complete, non-stitched replies written for the setting. Ten percent of replies may open with a current-personality tone line from a 60-line personality pool. When a matching real work fact exists within the previous seven days, about 30% of eligible replies may mention it. Personality visibly weights reply types.

Five mutually exclusive emotion states come from serious talks and life events. They decay at sleep boundaries and never overwrite personality:

| Emotion | Trigger | Duration | Effects |
|---|---|---|---|
| Inspired | Praise or encouragement | 3 days | +15% work speed, +15% walk speed, +1 quality tier |
| Low | Being corrected | 1 day | -10% work speed, +18% walk speed, -1 quality tier |
| Relieved | Successful apology or sharing something difficult | 1 day | Guaranteed hug; blocks floor-sleep, late-night, illness, and missed-breakfast bond penalties |
| Heard | Listening to a complaint | 2 days | Blocks the same automatic bond penalties |
| Missing home | Not wanting to leave home | 1 day | No cross-map work; stays on the farm; guaranteed hug |

"Share something difficult" and "ask them to stay longer" end the conversation after the child's reply and apply their effect immediately. Before bed, a nearby child shows a gentle current-emotion line. Ordinary chat keeps only yesterday's important events and has a 30% chance to mention one with a light emotional touch, without overwriting the persistent emotion state.

"Ask about their learning progress" is a permanent read-only information page. It calls the average of real-player-parent bonds the child's "overall sense of security", shows their current speed as a rounded share of a player's normal run, and describes profession, exact profession level, work quality, personality, and growth in plain in-world terms. Each row explains what helps it and where it may be noticed, without exposing internal formulas, frames, pixels, dormant switches, or exact hidden values. Warriors also receive a low-resolution-safe page for their saved mine goal and a plain-language description of their combat growth. The page reads existing host facts and never creates another progression record or changes a random dialogue choice.

Probabilistic dialogue is fixed by date, child, parent, and dialogue meaning. Reopening the menu on the same day cannot reroll a new line; a repeated ordinary conversation instead shows "We have already chatted today; let's talk again tomorrow." Chinese informal dialogue addresses parents as 爸/妈, while English uses the actual player name. Cancelling the child menu gives that exact child/parent/screen a two-second pass-through window so doors, chests, and furniture remain usable.

### EN 9. Careers, farewell, and letters

Future careers are border guardian mage, city office worker, frontier warrior, game streamer, and farmer in another town. Each career has its own farewell lines; generic text such as "depart to become X" is not inserted into one shared sentence. Personality weights the line choice.

A confirmed departure is delayed if the child is not safely inside the recorded home at day end. After departure, exactly one deterministic-random real-player parent receives the first safe-arrival letter. An NPC parent can never receive it.

The first letter mentions that the child's backpack belongings were left "in that corner we both know." Each item is converted into a durable claim preserving qualified item ID, stack, quality, and tool upgrade level. The original backpack clears only after all claims are created. Opening the linked letter unlocks delivery of those claims to the selected player parent.

Departed children stay in touch. For each child, festival instance, and year, one letter is sampled per festival: 15% a profession-specific letter, otherwise 25% a general letter, and 60% no letter. Both letter types always carry a surprise gift. Every festival has five random texts per type, covering the festival, caring for the parents, and the child's farm profession as a hobby. Birthday letters arrive with a personal scene, a gift, and 1,000,000 gold. The child may also send small allowance letters with a recipe note.
### EN 10. Festivals, routes, and rain

Festival days keep normal work, conversations, gifts, storage returns, sleep, and parent commands active. Festival-specific parent-proximity speech has priority. The safety rule applies only when a child tries to leave the farm domain: the child returns to the farm and patrols there until a parent command or an ordinary time transition takes over. Children are not forced onto the festival map and are not held at one fixed farm tile.

Movement across maps is handled by one public travel system. It plans only the next map's real entrance, walks the current map with the player-sized body, revalidates the entrance, and commits one native character warp. Building doors (barns, coops, greenhouses, farmhouse, cabins) are interaction targets: the child walks beside the door, faces it, and commits one entry action rather than stepping on a tile warp. Returning home always targets the child's recorded residence instance.

Static route passability follows the player contract: a static tile a player can stand and walk on must also be available to a child. Small pickup objects such as truffles, forage, weeds, breakable stones, and twigs may be ignored after blocking the same child for half a second, with a brief sidestep visual; terrain, machines, chests, walls, and buildings never use that exception. Warriors inside the mine use the mine tool transactions instead.

Route blocking is bounded: a child whose route stays blocked (for example by a player or NPC standing in the way) re-plans up to eight times and, only after all attempts fail, uses one fallback teleport into the destination map. The fallback is a last resort that logs an error with full details; it is never used on festival days and never replaces ordinary waiting or re-planning.

Rainy days weight children toward staying home. A child who cannot reach the chosen leisure spot degrades gracefully to another legal activity instead of standing still or teleporting.

### EN 11. Health, breakfast, and daily care

Children can wake up with one of five illnesses: cold, fever, stomachache, cough, or indigestion. The base chance is 1%, plus 2% when work was not finished before 18:00 and 3% when the child returned home at night. An illness can last up to seven penalty mornings; without the right food it clears naturally on the eighth day.

Every one of the 81 unlockable cooking recipes belongs to exactly one illness's cure list. Putting the correct food into the child's backpack cures the illness instantly: one item is consumed and the child starts one low-mood day. When no cure food is unlocked, talking to Harvey (the doctor) gives a warm line and mails one main cure food for that illness, at most once per illness episode.

Breakfast is attempted once per day at a random time between 08:30 and 09:30. Only adult children take part: babies and younger children never receive a missed-breakfast notice or bond penalty. The child picks one cooking or edible artisan item from the backpack, preferring smaller stacks, and consumes it. A missed breakfast costs 200 bond points. The actually eaten item also adjusts the result by its single-item sell price: under 200g costs 25, 200-499g is neutral, and 500g or more gains 25. There are 50 texts for each of the four tiers (none, low, mid, high), read from the translation files.

### EN 12. The warrior: mining and combat

"Plan a mine trip together" lets either recorded real-player parent save Copper (30-39), Iron (40-50), Gold (80-101), Coal (40-61), or Random for a warrior. Random is also the default and starts from a random legal level for the commanding parent, working down to the bottom before rerolling. Fixed ranges are repeatable rotations: after reaching the end floor, the child leaves through the real mine exit and, if work time remains, immediately starts the next round from the legal entry elevator. The chosen parent receives only progress that the child physically reaches through the real elevator, ladder, floor, and mine-entry rules; no option unlocks floors in advance. When the warrior formally enters a mine floor from the lobby, the commanding parent sees one "the child entered floor X" notice per trip.

The warrior uses a real melee weapon from the backpack, keeping native range, facing, weapon type, speed, critical chance, forge and enchantment effects, monster defense and evasion, dagger bursts, club special attacks, and the sword defense special. Child damage is the native weapon result multiplied by:

`multiplier = clamp(0.8 + 0.2 × skillRatio + 0.2 × bondRatio, 0.8, 1.2)`

with `skillRatio = clamp(professionLevel / 10, 0, 1)` and `bondRatio = clamp(averageRealPlayerBond / 2500, 0, 1)`. The final critical chance adds a fixed 30 percentage points over the real weapon's native critical chance, capped at 100% - the warrior's "keen sixth sense" talent.

Damage taken by the child is reduced by the defense bonus of the boots the warrior is wearing, through the native formula: each defense point subtracts 1 from the rolled damage, the result never drops below 1, and defense at least half the damage decays by a small random amount. The warrior's learning-progress page shows the current defense value.

Each trip starts at 250 HP. When a real player is on the same actual mine floor, monsters can target and hit the child through their real hitboxes; every valid hit costs 5 HP and all monsters share a two-second child-damage cooldown. Without a real player on that exact floor, monster AI stays dormant and entering the floor costs 25 HP once. At five HP or below, the child consumes at most one native healing food per recovery step, choosing the least-wasteful item which leaves the danger line or otherwise the strongest heal. If no healing food remains, the trip ends, the child returns through the recorded home entrance, and one fixed incident report waits for the commanding player parent or another available real parent. HP is host-owned; each screen only renders the same soft low-health red pulse and never recalculates damage.

If a recorded real parent enters an actual Skull Cavern floor, their warrior stops ordinary work or leisure and joins that parent through a legal nearby landing with a shared teleport effect and one fixed arrival line from a five-line pool. The warrior follows the same parent across real floor changes, stays within five tiles when possible, and uses a fixed priority: own-range threats first, threats around the parent (especially behind), valuable ore within five tiles, breakable stone, then a back-to-back guard stance with the real weapon. When the warrior takes up the guard, it holds the sword's ready frame statically - a presentation fact, not a second combat state. During the rescue, thirty extra warrior-specific bubble lines can appear. When every real parent has left Skull Cavern, the warrior returns once through the recorded home entrance and the supported parent receives a local notice. Sleep is never interrupted. Choosing "Don't work too hard, take a few days off" stops all professional activity, including an active mine trip or rescue, until a parent explicitly starts work again.

### EN 13. Settings page, shared time flow, and leisure mode

The backpack menu has its own "GCE settings" tab (independent of UIInfoSuite2). It shows online player birthdays, the leisure-mode toggle, and the safe-removal star. The page works with mouse and gamepad; one A-press produces exactly one click. The tab is anchored to the actual panel width, so it no longer floats over the edge on narrow inventory pages.

- **Birthdays.** Each online real player picks their own season and day 1-28. Settings are saved to the host's `ParentBirthdays` configuration and broadcast; remote players may change their own entry. Each player may change their own birthday once per year; the choice reopens every Spring 1, and a second change in the same year is rejected with a notice.
- **Leisure mode.** Host only. The host may start it at most twice per week (the count resets on Monday morning). Starting immediately pauses the shared game clock; the host may end it manually, it ends at the third violation by any player, and it always ends before saving or at a new-day boundary. It cannot be started on a festival day. During leisure all awake children stop work and choose a free activity (home leisure, Saloon, town, beach, forest, or mountains) instead of standing still; when the mode ends they resume the plan they had before. Tool commits and backpack changes count as violations; giving food to a child, gifting between players, furniture placement, and ordinary menu-paused exchanges are exempt. Switching lists or tabs while a menu is open - including gamepad shoulder-button page changes - is exempt because it is a menu-paused operation. Keg removal/insertion and fishing casts each count once (player-confirmed ruling). Everyone sees the custom 16x16 tea icon in the corner and localized notices.
- **Safe removal.** Host only. See the safe-uninstall section below.

Shared time flow affects only the game clock, never characters, machines, animations, lighting, or nightfall. In multiplayer, each online player who would pause time in single-player contributes an equal share (two players 50% each, three 1/3, four 25%) while a menu is open. Partial pause (for example one of two split-screen players opening a menu) keeps the time digits blinking at twice the full-pause blink rate. Native slow-time areas (currently the Skull Cavern, which extends each in-game minute by 200 ms) apply to the shared clock when any online player is inside. `PauseInMultiplayer` may coexist, but its running logic is suppressed while GCE owns the clock.

### EN 14. Safe uninstall

Growing Children's own `gc_uninstall` is not used for an Expanded save: it mutates immediately, covers only the current player, and has no verified transaction for adult projections or departed children. Safe removal therefore prepares the complete stack for removal.

On the GCE settings page, safe removal is a five-statement star. Checking a statement draws a line; all five light the outer ring and queue the request. Unchecking all five cancels a request that has not been saved yet. The current session is never changed immediately: the request is committed only at the next normal sleep save, which restores every active and departed child as a visible vanilla child, clears both mods' behavior data, adult NPC/agent/cache residue, and child friendship residue, and validates the household before marking the save prepared. A small inert recovery ID remains on each restored child. Unchecking before either folder is deleted cancels the request; a request already committed at the sleep save is irreversible.

After the next morning's confirmation, quit completely and remove both `GrowingChildrenExpanded` and `GrowingChildren`. Removing only Expanded is not supported. If both mods are installed again and the matching combined backup still exists, the host may restore the family from GMCM. The backup lives only in GCE's own save namespace; deleting it removes recovery eligibility but never changes current vanilla children.

Turning a child into a dove through the Dark Shrine uses the original `Farmer.getRidOfChildren()` boundary: the matching adult NPC, work agent, caches, pending child mail/rewards, relationship residue, and durable Expanded state are retired in the same transaction. Vanilla dove behavior is preserved with no next-morning resurrection.

### EN 15. Testing boundary

A successful build proves only that the source compiles. A clean SMAPI launch proves only that patches load. Movement, water routing, beds, floor sleep, carrying hugs, NPC-parent hugs, television competition, serious-talk recovery, emotion states, illness and breakfast, remote parent requests, departure mail, festival letters, boots wear and backpack auto-wear, prismatic hair, native growth-stage sliders, growth scaling, sleep-tip display and duration, the yearly birthday limit, the route fallback teleport, the breakfast age gate, seed-exhaustion planting, the settings-tab anchoring, gamepad settings clicks, mining combat, Skull Cavern rescue, shared time flow, leisure mode, and uninstall restoration require a real save test. Release notes keep those boundaries explicit.
## 中文维基

### ZH 1. 需求与安装

Growing Children Expanded 需要星露谷物语 1.6、SMAPI 4.0 或更高版本、Growing Children 1.0.2 以及 SpaceCore 1.28.4 或更高版本。Generic Mod Config Menu（GMCM）是可选项。

请保留原版 Growing Children 模组，把本扩展作为 `Mods` 下的独立文件夹安装。更新时替换扩展文件，但保留你现有的 `config.json` 以沿用设置。

安装 GMCM 后，扩展页面会同时接管原模组的四项设置：幼儿长大前等待天数、自动进入长大流程、孩子数量上限和原模组调试日志。已有原模组数值只导入一次，然后桥接到每个本地原模组实例。单纯启动游戏或部署更新不会改写 `config.json`；只有保存 GMCM 或明确更改安全卸载状态才会写入。

同一个 GMCM 页面还提供原生四个成长阶段（摇篮、婴儿、爬行、学步天数；默认 7/7/7/7，范围 3 天到原生时长两倍）和睡前小提示（总开关与“每天/偶尔”频率）的设置。

### ZH 2. 身份、父母与多人

每个孩子都拥有持久的身份 ID。名字、临时 NPC 实例、头像、本地分屏画面和远程客户端都只是这个身份的呈现，而不是独立的孩子。

存档变更由主机唯一提交。本地分屏副手在同一进程内把输入交给主机；远程副手发送定向请求，主机验证发送者是记录的真人父母后只提交一次，再广播状态并把父母专属提示发给正确的玩家。

NPC 配偶也可以记录为 NPC 父母。他们能参与合适的生活互动，例如孩子走过去拥抱；但他们永远不会收到玩家关系账本、邮件、奖励、配置提示、床位询问或种植区询问。

家庭也会随时间变化。离婚后孩子保留自己的历史：约一季（28 天）内可能出现悲伤或困惑的对话，羁绊增长暂时减半。旧父母保留历史身份，但停止对日常数值的继续写入。再婚后新配偶从 0 开始建立关系，不继承旧关系点。

### ZH 3. 成长、移速与性格

成长是渐进的：刚长大的孩子从普通成人体型约 50% 开始，按配置的成长年数平滑过渡到成年和成熟体型。成长共分十档，体型每增加 10% 到达一档；每档只在首次到达的早晨提示一次，文案符合孩子的性格，重复载入不会重复提示。

原生婴儿使用独立曲线：在每个原生阶段（摇篮、婴儿、爬行、学步）中，孩子从基础贴图的 50% 慢慢长到 70%，长大日恰好达到 70%，时间轴等于四个配置阶段之和。成年过渡从 50% 长到 100%；成熟期男孩两维都长到 110%，女孩身高 105%、宽度保持 100%。性格体型因子（活泼 1.08 / 独立 1.04 / 随和 1.02 / 好奇 0.98 / 沉静 0.96 / 细心 0.92）在所有阶段生效。

六种性格（活泼、独立、好奇、随和、沉静、细心）会影响体型、走路、工作停顿、对话权重以及各羁绊来源的强弱。性格乘数在来源间轮换平衡，一个性格在某项活动上的优势会被其他活动的较小收益抵消。

移速基础为玩家无增益普通跑步速度的 100%，再叠加成长、性格和羁绊因素。孩子实际跨图工作、回家或去晚间活动时，基础暂时变为 120%；到达目的地地图后立即恢复普通工作或休闲基础。战士往返矿井的整段行程使用 150%。晚间室内休闲与已到达的休息地点使用 60%，餐吧休闲使用 40%。骷髅洞穴救援追赶时，速度随距离连续提高，安全上限为 250%。

`羁绊速度系数 = 0.80 + 0.25 × t`

其中 `t = 真人父母平均羁绊 / 2500`，限制在 `0..1`。最低羁绊因此为 `-20%`，最高羁绊为 `+5%`。NPC 父母没有玩家账本，不参与平均。

移动所有权附着在权威孩子实例及其路线控制器上，不通过周期性位置或速度镜像修复。

### ZH 4. 亲子羁绊

羁绊范围是每位真人父母 `0..2500`。一颗心为 250 点，半颗心为 125 点。提示使用温暖、生活化的语言，并在结尾用括号显示实际数值；即使实际应用为零，也会显示请求值（例如没吃早饭仍会显示 `-200`）。羁绊满值时显示 `(∞)` 并附一句温馨表达，而不是普通的零。

| 事件 | 基础变化 | 限制 |
|---|---:|---|
| 每日普通聊天 | +50 | 每个孩子、每位真人父母每天一次 |
| 玩具陪伴到损坏 | +250 | 玩具事务完成时；归给记录中的赠送者 |
| 睡在地上 | -125 | 每天对每位真人父母一次 |
| 季度换衣或换发色 | +125 | 每季度、每个孩子、每位操作父母一次 |
| 有合适的床 | +10 | 每天对每位真人父母一次 |
| 工作完成后的想念时刻 | +20 | 确定性 20% 机会；无论实体拥抱是否成功都提交 |
| 送出惊喜礼物 | +250 | 惊喜礼物事务提交时 |
| 早餐 | -200 / -25 / +25 | 每天一次；详见健康一节 |

性格会在这些来源之间轮换平衡。所有扣减都保存成独立损失记录，因此道歉只能找回最近真正扣掉的数值，不会凭空产生羁绊。

“认真谈谈”中的道歉会处理最近两天内全部真实、尚未恢复的扣分。每个失败记录保留，并继续受自己的两天有效期限制；只有成功才把对应扣分标记为已恢复。没有候选时不显示任何道歉结果文案。

### ZH 5. 工作、工具与产物品质

孩子保留原来的农场职业：农夫、动物照料员、渔夫、采集者和战士（内部键仍为 `Mineiro`）。工具动作把已提交外观、工具等级、朝向、目标格、挥动帧与停顿时间作为一个表现事务。

农夫把锄地、浇水、播种、施肥和收获分别作为原生农作动作发现与提交；施肥是独立的原生目标，会在农夫结束当日农活前找到。种子用完时，农夫会就地裁剪剩余的播种计划并立刻进入浇水，不再走到空地块上一格一格做空播种动作。采集者每天规划最多 25 个可达目标，并在主机把产物放入背包、移除精确来源之前重新核对每一棵杂草、树枝、石头、采集物或远古斑点；农场采完后可以跨到一张随机、合法、非农场的地图继续采集。渔夫会保存父母选定的地点（或每次行程开始选一个新的合法随机地点），用背包中的真实鱼竿抛竿，显示鱼线和浮标，等待 10–30 个游戏分钟后收杆；鱼竿等级越高，最长等待越短。只有背包真正接收一份当前水域的原生鱼获，才增加捕获数。

产物品质先由成长阶段和职业技能等级决定基础档，当前等级内的 XP 最多提供 25% 的升一档概率，最后应用情绪品质 `+1/-1` 并在原生阶梯 `普通 -> 银 -> 金 -> 铱` 内夹紧。银从工作第 8 天解锁，金从第 15 天解锁。铱星从第 21 天（满羁绊）到第 112 天（最低羁绊，即一整年）之间按下列公式解锁：

`铱星解锁日 = ceil(112 - 91 × t)`

只有生产时刻已提交的羁绊值生效；已生成物品不会追溯升级。早餐吃下的食物可以把当天剩余时间的职业等级临时提高（农夫/动物照料员用 Farming，渔夫用 Fishing，采集者用 Foraging，战士用 Mining），最高计至 10 级。

每个孩子每个工作日只进行一次互斥工作事件抽样，在 12:00 之后到当天结束之间选择随机时间：2% 工作意外、2% 获得宝物、96% 无事件。每种职业准备 5 条意外文案和 5 条宝物文案。工作结束进入农舍时，50% 概率显示“我回来了”一类头顶气泡，只有 5 格内能看到气泡的父母可见。背包已满时，孩子会主动向父母汇报，不会默默丢失物品。

羁绊会调整工具动作之间的停顿：

`羁绊停顿系数 = 1.50 - 0.80 × t`

最低羁绊 `+50%` 停顿，最高羁绊 `-30%` 停顿，再乘以性格系数。

#### 归箱与动物反馈

“把东西放回去”是主机拥有的事务。孩子会访问支持的普通箱子、大箱子和冰箱，但只加入已有且未满的兼容堆叠；空格永远不会新建堆叠，因此不同物品可能放进不同容器。一次成功的实物容器访问会产生一次开盖动画和一条父母 HUD 提示，列出实际放入的物品。失败、不匹配或容量受阻的堆叠仍留在背包，绝不会被报告为成功。

所有真实工具（包括水壶、奶桶、剪刀、鱼竿和武器）都会留在孩子背包，仍有效的珍贵玩具也受保护。动物照料由主机提交一次；观察屏只展示叫声/表情或越栏跳跃。体型越大的动物跳得越低，但不会改变其世界位置或 AI。
### ZH 6. 外观、宝石染发与靴子

发型和服装变化提交到孩子身份，再被世界精灵、菜单、头像、工作动画、传送和分屏画面使用；帽子、上衣和裤子属于同一套已提交外观。工具动画不会从更早的发型状态重建。

可识别的染发宝石包括普通宝石和钻石。钻石产生接近白色、略带蓝调的颜色：RGB `242, 248, 255`。五彩碎片是特殊礼物：父母把它放进背包并关闭背包后，孩子会按性别随机选一个新发型和随机发色，并用十条随机回复之一回应。

背包首次持久账本迁移会识别已有的染发宝石和玩具，而不是把所有物品重新标记为通用系统奖励。只有真人玩家在本次背包会话中亲自放入、真实新增且符合古物契约的古物才会成为珍贵玩具；工作采集到的古物永远是工作成果，绝不是玩具来源。

靴子是继帽子、上衣、裤子之后的第四个穿脱槽位。手持任意靴子与孩子互动即可穿上：手中的靴子被消耗，如果孩子原本穿着靴子，旧靴会返还到你的物品栏。孩子菜单的穿衣页可以穿脱全部四个槽位（穿上消耗你物品栏中的对应物品，脱下返还物品）。孩子在世界和工作动画中的外观按所穿靴子的颜色着色；旧存档的鞋色只在未穿靴子时作为兜底颜色显示。父母把帽子、上衣、裤子或靴子放入孩子背包并关闭背包时，如果对应槽位为空，孩子会自动穿上：物品从背包与账本消耗，孩子用一条温馨回复回应，赠送者获得每季度一次 +125 的外观羁绊。工作收获的物品永远不会触发自动穿。

### ZH 7. 家庭生活、床、拥抱与电视

成年孩子只结算一份记录的住所和床位事务。有合适的床每天 +10 羁绊；没有可用床时睡在地上并损失半颗心。地铺使用清晰的无阻挡 3x2 占地，尽量靠近记录中的父母，有可见的枕头和被子（取默认床材质与浅土黄色条纹）。整套地铺绘制在玩家和重叠家具下方。

每个睡着的孩子每 10 个游戏分钟更新一次视觉睡姿：侧卧 80%，平躺 15%，头朝下露出后脑勺 5%。只切换孩子自身的前、侧、后视图；床、枕头和被子不会旋转。

原版幼儿完整保留前往 `GetChildBedSpot` 的上床路线、抵达回调、睡前随机玩具动画、按年龄的绘制缩放和原生床锚点。原生睡眠提交后，扩展只在孩子保持睡眠期间阻止竞争动画写入；醒来立即把动画所有权交还游戏。

电视是日常流程的一部分。晨间电视在真正开屏后驻足 5 个游戏分钟，并读取真人父母当天的真实运势，使用游戏当前语言的原生运势术语；晚间电视驻足 30 个游戏分钟，报告次日天气。白天居家休闲时孩子可能改看生活、烹饪或钓鱼节目，不会重复晨间运势或晚间天气。同一住宅、同一时段由第一个成功取得电视预约的孩子观看，其他孩子立即跳过，不排队。所有真人父母都不在时，孩子可以把电视留着当屋里声音；这台环境声电视不占用观看权，之后的孩子会直接切换节目。

拥抱沿用既有概率与冷却门禁。孩子先选择同地图最近的合法真人父母；同地图没有父母时，可以通过公共旅行系统接近其他在线父母。玩家生日当天和雷雨夜睡前拥抱 100% 触发。手举物永远不会被复制、移除或重新插入；拥抱结束后会恢复槽位、精确物品引用、朝向和手举展示。孩子也可以走近记录中的 NPC 父母进行纯视觉拥抱，不产生羁绊、邮件或奖励。两个成年孩子同图两格内相遇且均不在冷却时，主机 100% 创建一次相遇：双方停下面对面交谈，谈话结束后由原所有者继续。一方正在工作或赶路时，80% 使用忙碌短句完成相遇。

**睡前小提示。** 睡觉换日时，模组会显示一条小提示并贯穿整个全黑窗口：在“正在储存/进度已保存”黑屏页和结算主页面都保持可见，过渡结束才随黑屏消失；打开结算详细清单会立即隐藏。单人模式提示位于屏幕底部正中间，最低点与原生“正在储存/进度已保存”文字对齐，中间一个字对准屏幕中心；本地分屏时，每个画面都把它放在共享窗口接缝的中心。主机每晚抽取一条（避开最近显示过的），所有画面显示同一条。最多 8 条额外提示可排队，每晚显示一条。46 条中英双语提示池覆盖容易忽略的 GCE 机制；功能与频率（每天或约隔天一次）都可配置。

### ZH 8. 对话、认真谈谈与情绪

孩子菜单保留全部原选项。“认真谈谈”出现在“以后再聊”之前，包含：

- 道歉
- 夸奖
- 责备
- 鼓励
- 听抱怨
- 分享不愉快经历
- 问问学习进度
- 让成熟的孩子多留一会儿（可用时）
- 返回

道歉有 20 条带性格标签的温馨回复。每个孩子、每位真人父母每天一次，主机做确定性 50% 判定；成功时恢复最近两天内真实、尚未恢复的扣分，规则见羁绊一节。

每个认真谈谈选项都有完整、非拼接的回复池。10% 的回复会以当前性格的基调句开头（六种性格各 10 条，共 60 条）。前七天存在匹配的真实工作事实时，约 30% 的相关回复会提及它。性格会明显影响回复类型的权重。

五种互斥的情绪状态来自认真谈谈和生活事件，睡醒时衰减，永远不覆盖性格：

| 情绪 | 触发 | 持续 | 效果 |
|---|---|---|---|
| 受鼓舞 | 夸奖或鼓励 | 3 天 | 工作速度 +15%、走路速度 +15%、品质 +1 档 |
| 低落 | 责备 | 1 天 | 工作速度 -10%、走路速度 +18%、品质 -1 档 |
| 释然 | 道歉成功或分享不愉快经历 | 1 天 | 保证拥抱；阻止睡地板、夜外滞留、疾病、缺早餐的自动扣羁绊 |
| 被倾听 | 听抱怨 | 2 天 | 阻止上述自动扣羁绊 |
| 思念 | 舍不得孩子离家 | 1 天 | 不跨图工作，只在农场活动；保证拥抱 |

“分享不愉快经历”和“多留一会儿”在孩子回复后立即结束对话并生效。睡前靠近孩子时，会显示生活化的当前情绪提示。普通聊天只保留昨日的重要事件，并有 30% 概率顺带一条情绪表达，不会覆盖或刷新持久情绪状态。

“问问学习进度”是常驻只读信息页：显示真人父母平均羁绊的“综合安心值”、当前移速占玩家普通跑步的百分比范围、职业、确切职业技能等级、性格倾向、物品品质加成和成长信息，全部使用符合游戏气氛的白话。战士额外有一页保存的下矿目标和战斗成长说明。该页只读取既有主机事实，绝不建立第二份成长记录，也不改变随机对话选择。

概率对话按“日期 + 孩子 + 父母 + 语义”固定；同一天重复交谈不会刷出不同文案，重复普通聊天会提示“今天已经聊过啦，我们明天再聊吧”。中文对话用 爸/妈 称呼父母，英文使用玩家名字。取消孩子菜单会给该孩子/父母/屏幕一个两秒穿行窗口，方便操作门、箱子和家具。

### ZH 9. 职业、告别与信件

未来职业包括边境守护法师、大城市职员、荒野开拓战士、游戏主播和异乡农场主。每种职业都有独立的告别台词，不会把通用“去成为 X”塞进同一句话；性格会加权台词选择。

确认离家后，如果孩子当天结束时不在记录住所的安全范围内，离家会延迟。离家后恰有一位确定性随机真人父母收到第一封平安到达信；NPC 父母永远不会收到。

第一封信会提到孩子的背包物品放在了“那个我们都知道的角落”。每件物品都转换为保留完整 ID、堆叠、品质和工具升级等级的持久认领；原背包只在这些认领全部建立后清空。打开关联信件即可把这些认领交付给选定的真人父母。

离家的孩子会保持联系。每个孩子、每个节日实例、每年只抽一次信：15% 职业特殊信，未中再抽 25% 通用信，其余 60% 不发信。两类信都附惊喜礼物，每个节日每类有 5 条随机文案，内容涵盖节日、关心父母和孩子仍爱好的农场职业。生日信带个人场景、礼物和 1,000,000 金币。孩子还可能寄出带食谱备注的零花钱小信。

### ZH 10. 节日、路线与雨天

节日当天日常工作、聊天、礼物、归箱、睡眠和父母命令照常，节日专用的父母邻近对话有优先权。安全规则只在孩子试图离开农场域时生效：孩子返回农场并巡逻，直到父母明确指令或正常时间转换接管。孩子不会被强制送去节日地图，也不会被固定在一个农场地格。

跨图移动由同一套公共旅行系统完成：只规划下一张地图的真实入口，在当前地图按玩家大小身体走路，抵达入口重新核对，再提交一次原生角色传送。畜棚、鸡舍、温室、农舍和 Cabin 的人门属于交互目标：孩子走到门旁、面向门并提交一次进入动作，而不是踩上自动换图。回家始终锁定孩子记录的住所实例。

静态路线可达性以玩家契约为准：玩家能站立和行走的静态格，孩子也必须能到达。松露、采集物、杂草、可破坏石头和树枝等小拾取物连续挡住同一孩子半秒后可以临时忽略，并显示短促侧身视觉；地形、机器、箱子、墙和建筑不适用。矿井内的战士改走采矿工具事务。

路线受阻有明确上限：孩子的路线持续被挡（例如玩家或 NPC 站在路上）时，会重新规划最多 8 次；全部尝试仍然失败后，才允许一次兜底传送进入目的地地图。兜底传送是最后手段，日志中标为错误并输出详细信息；它绝不会在节日触发，也不会替代普通的等待或重规划。

雨天孩子更倾向在家休闲；无法到达选定休闲地点时，会降级到另一个合法活动，而不是站着不动或传送。
### ZH 11. 健康、早餐与日常照料

孩子可能患五种病之一：感冒、发烧、胃痛、咳嗽、积食。基础概率 1%；前一天 18:00 前仍未结束工作加 2%，夜晚未回家加 3%。疾病最多产生七个惩罚早晨；没有正确食物时第八天自然痊愈。

全部 81 道可解锁烹饪菜品都被完整划分到五种疾病的治愈食物。父母把对应食物放入孩子背包会立即治愈：消耗一件，孩子进入一天低落。未解锁任何治愈食物时，找哈维医生聊天会得到一条温馨文案，并邮寄一份该病的主力治愈食物；同一发病期只寄一次。

早餐每天在 08:30–09:30 之间随机时间只尝试一次，而且只适用于成年孩子：婴儿和更小的孩子永远不会收到“没吃早餐”提示或扣羁绊。孩子从背包随机选一件烹饪物品或可食用工匠品并消耗，优先选择堆叠较少的物品。没有早餐扣 200 点羁绊。按消耗物单件实际售价修正结果：售价低于 200g 扣 25，200–499g 不变，500g 以上加 25。无食物、低价、中价和高价四档各备 50 条文案，从翻译资源读取。

### ZH 12. 战士：下矿与战斗

“一起规划下矿目标”可让任一记录的真人父母保存铜矿石（30–39 层）、铁矿石（40–50 层）、黄金矿石（80–101 层）、煤炭（40–61 层）或随机。随机也是默认值：从对下达父母合法的一个随机层开始，向下工作到底层后再重抽。固定范围是可重复的完整轮次：到达终点层后，孩子经真实矿井出口退矿；若当天还有工作时间，立即从合法进入层开始下一轮。下达指令的父母只获得孩子通过真实电梯、梯子、楼层和矿井入口亲自抵达的进度；菜单不会提前解锁楼层。战士每次从大厅正式进入矿层时，目标父母会收到一次“孩子进入了第 X 层”的提示。

战士使用背包中的真实近战武器，保留原生范围、朝向、武器类型、速度、暴击、锻造/附魔、怪物防御与闪避、匕首连续突刺、锤类特殊攻击和剑类防御技能。孩子伤害是原生武器结果乘以：

`倍率 = clamp(0.8 + 0.2 × 技能比 + 0.2 × 羁绊比, 0.8, 1.2)`

其中 `技能比 = clamp(职业等级 / 10, 0, 1)`，`羁绊比 = clamp(真人父母平均羁绊 / 2500, 0, 1)`。最终暴击率在真实武器原生暴击率之上固定增加 30 个百分点，上限 100%——这是战士“敏锐第六感”天赋。

孩子受到的伤害会按所穿靴子的防御加成走原生减伤公式：每点防御从滚动伤害中减 1，结果最低为 1；防御至少为伤害一半时，防御值还会按原生规则小幅随机衰减。战士的“关心学习进度”矿工页会显示当前防御值。

每次行程从 250 点生命开始。同一个实际矿层存在真人时，怪物会通过真实命中框选择并命中孩子；每次合法命中扣 5 点，所有怪物共享两秒受伤冷却。该层没有真人时怪物 AI 休眠，孩子每次实际进入该层固定扣 25 点。生命不高于 5 时，孩子每个恢复步骤最多吃一份原生可食用回血物品，优先选择能脱离危险线且浪费最少的一份，否则选择恢复量最高的一份。没有回血食物时结束行程，经真实住所入口回家，并把同一份遇险事实汇报给下达指令的父母或另一位可用真人父母。生命只由主机结算，各屏只显示柔和低血红光。

任一记录的真人父母进入实际骷髅洞穴层时，战士会停止普通工作或休闲，从附近合法空格赶来（带一次共享传送特效和五选一固定到达台词），跨真实换层跟随同一位父母，尽量留在五格内。救援优先级固定：自身攻击范围内的威胁、父母身边（尤其背后）的威胁、五格内的高价值矿、可破石，最后是与父母背对背的持械警戒站位。警戒只用真实武器的准备帧做静态展示，不触发真实攻击或格挡。救援期间额外出现 30 条战士专属头顶气泡。所有真人父母离开后，战士经真实住所入口安全回家，被支援的父母收到一次提示。睡眠从不被打断。选择“别太累了，休息几天”会原子停止孩子的一切职业活动（包括进行中的矿井行程或救援），直到父母再次明确开始工作。

### ZH 13. 设置页、共享时间流速与休闲模式

背包菜单新增独立的“GCE 设置”标签页（不依赖 UIInfoSuite2），包含在线玩家生日、休闲模式开关和安全卸载五角星。页面支持鼠标和手柄，一次 A 键只产生一次点击。标签页按面板真实宽度锚定，窄页上不再悬空浮出右缘。

- **生日。** 每位在线真人玩家分别选择自己的季节和 1–28 日，保存到主机的 `ParentBirthdays` 并广播；远程玩家可以修改自己的条目。每位玩家每年只能修改一次自己的生日，春 1 恢复修改资格；同一年内第二次修改（包括设为“无”）会被拒绝并给出提示。
- **休闲模式。** 仅主机可操作。主机每周最多启动两次（周一早晨重置计数）。开启立即暂停共享游戏钟；主机可以手动结束，任意玩家累计第三次违规自动结束，保存前或换日边界也必定结束。节日当天不能开启。休闲期间所有清醒孩子停止工作并自由活动（居家休闲/餐吧/镇上/海边/森林/山上），不会站着不动；模式结束时恢复进入前的计划。工具提交和背包变化算违规；给孩子放食物、玩家互赠、家具摆放和菜单暂停的交换过程豁免。菜单打开期间的列表/页签切换（含手柄肩键切页、切换分类或物品栏页）属于菜单暂停操作，一律豁免。从小桶取放和钓鱼抛竿各记一次违规（玩家实测裁决）。所有在线玩家都能看到角落的自绘 16x16 茶杯图标和本地化提示。
- **安全卸载。** 仅主机可操作，详见下一节。

共享时间流速只改变游戏钟，绝不改变角色、机器、动画、光照或黑夜判断。多人时，每位在单人模式会暂停时间的在线玩家贡献相等份额（两人各 50%、三人各 1/3、四人各 25%）叠加菜单暂停。部分暂停（例如双人同屏其中一人打开背包→半速）时，时间数字保留跳动特效，闪动速度为完全暂停时的两倍。原生慢速区域（目前是骷髅洞穴，每游戏分钟延长 200 毫秒）在任一在线玩家身处其中时作用于共享时钟。`PauseInMultiplayer` 可以共存，但 GCE 接管时钟期间会抑制其运行逻辑。

### ZH 14. 安全卸载

Expanded 存档不使用 Growing Children 自带的 `gc_uninstall`：它会立即修改、只覆盖当前玩家，也没有验证成年投影或已离家孩子的完整事务。

在 GCE 设置页，安全卸载是五条声明组成的五角星：勾选一条画一条连线，五项全选点亮外圈并排队；五项全部取消会撤销尚未保存的请求。当前会话绝不会立刻改变：请求只在下一次正常睡觉保存时提交——把所有在家的和已离家的孩子恢复为可见原生 `Child`，清理两套模组的行为数据、成年 NPC/代理/缓存和孩子好感残留，并在标记完成前核验家庭。每个恢复孩子只留下一个无行为的恢复 ID。删除文件前取消勾选会撤销请求；已在睡觉保存边界提交的请求不可撤销。

看到次日早晨确认后，彻底退出游戏，再同时移除 `GrowingChildrenExpanded` 和 `GrowingChildren`。当前不支持只卸载 Expanded。如果两个模组一起重新安装且匹配的合并备份仍在，主机可从 GMCM 恢复整个家庭。备份只保存在 GCE 自己的存档命名空间；删除它只清除恢复资格，绝不改变当前原生孩子。

通过黑暗神龛把孩子变成鸽子时，使用原生 `Farmer.getRidOfChildren()` 边界：同一事务退休匹配的成年 NPC、工作代理、缓存、待处理孩子邮件/奖励、关系残留和持久扩展状态。原版鸽子行为被保留，第二天早晨不会复活。

### ZH 15. 测试边界

编译成功只证明源码可编译；SMAPI 无报错启动只证明补丁可以加载。移速、水面路径、床位、地铺、手举物拥抱、NPC 父母拥抱、电视竞争、认真谈谈恢复、情绪状态、疾病与早餐、远程父母请求、离家信件、节日信、靴子穿脱与背包自动穿、五彩石发型、原生阶段滑块、成长缩放、睡前提示的显示与时长、生日年度限制、路线兜底传送、早餐年龄门槛、种子耗尽裁剪、设置页标签锚定、手柄设置页点击、采矿战斗、骷髅洞穴救援、共享时间流速、休闲模式和安全卸载恢复都必须用真实存档验证。发布说明会明确标注这些边界。
