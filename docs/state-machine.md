State machine notes

The engine is one pure function:

`State + Event -> State | Error`

State shape (idea)
The state must track:
- players
- roles (emperorPlayer, slavePlayer)
- hands (current decisive round hand by player)
- pending picks (optional by player)
- score (decisive wins by player)
- seed
- decisiveRoundIndex (0 to 12)
- decisiveInSide (0 to 3)
- isTieBreak (boolean)

Definitions:
- decisiveRoundIndex counts completed decisive rounds (wins only, not draws)
- decisiveInSide counts completed decisive rounds inside the current side
- isTieBreak is false during the main 12 rounds, true only during the extra round

States (V1)

Lobby
Before the match starts.

Valid events:
- Start(seed)

Transition:
- Start -> RoundSetup

RoundSetup
Prepare the next decisive round.

Rules:
- If decisiveRoundIndex is 0, pick initial roles from the seed.
- Hands reset here for the new decisive round (full hand based on current roles).
- Pending picks are cleared.

Side switching rules:
- After a decisive outcome, if decisiveInSide becomes 3 and isTieBreak is false:
  - switch roles
  - set decisiveInSide back to 0
- Otherwise roles stay the same.

Tie break start rule:
- If decisiveRoundIndex is 12 and isTieBreak is false:
  - if score is tied, set isTieBreak to true and prepare the extra decisive round
  - if score is not tied, the match should end

Valid events:
- BeginPicking

Transition:
- BeginPicking -> Picking

Picking
Both players can submit exactly one pick (secret).

Valid events:
- Pick(player, card)

Rules:
- Pick is valid only in Picking.
- Pick is valid only if the player has not picked yet for this reveal.
- Pick is valid only if the card is currently in that player's hand.
- Picks are private until Reveal.

Transition:
- When both players have a pending pick -> Reveal

Reveal
Reveal both picks, compute outcome, update hand and counters.

Card outcome rules:
- E beats C
- C beats S
- S beats E
- C vs C is a draw

Update rules:
- Remove the used cards from both hands.
- If draw:
  - score does not change
  - decisiveRoundIndex does not change
  - decisiveInSide does not change
  - go back to Picking (same decisive round continues)
- If decisive:
  - increment the winner score
  - decisiveRoundIndex += 1
  - if isTieBreak is false, decisiveInSide += 1
  - the decisive round ends

Transitions after a decisive outcome:
- If isTieBreak is true -> Finished
- If decisiveRoundIndex is 12:
  - if score is tied -> RoundSetup (tie break round, with isTieBreak set to true)
  - otherwise -> Finished
- Otherwise -> RoundSetup

Finished
Match over.

Winner rule:
- the player with the higher score wins
- (if tie break happened, it cannot end tied)

Valid events:
- Reset

Transition:
- Reset -> Lobby

Events (V1)
- Start(seed)
- BeginPicking
- Pick(player, card)
- Reset

Errors (minimum set)
- InvalidEvent
- WrongPhase
- UnknownPlayer
- AlreadyPicked
- CardNotInHand