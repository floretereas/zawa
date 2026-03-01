Rules notes (E-Card)

Quick summary of the rules I am implementing.

Cards

Emperor (E)

Citizen (C)

Slave (S)

Roles per round
Each decisive round has two roles:
one player is the Emperor
the other player is the Slave

Hands by role (used inside a decisive round):

Emperor hand: 1x E and 4x C

Slave hand: 1x S and 4x C

A new decisive round starts with a fresh full hand for the current roles.

One decisive round
Both players pick one card in secret, then reveal at the same time.

Card outcome rules:

E beats C

C beats S

S beats E

C vs C is a draw

Round rule:

a decisive round ends only when there is a winner

if the reveal is a draw (C vs C), the round continues:

both used cards are removed

roles stay the same

players pick again

Winner rule:

the winner is the player who played the winning card

Note:

E vs E and S vs S cannot happen in a valid reveal because roles differ.

Match flow (V1)
The match has 12 decisive rounds total.
It is split into 4 sides of 3 decisive rounds each.

side 1: decisive rounds 1 to 3 (initial roles)

side 2: decisive rounds 4 to 6 (roles switched)

side 3: decisive rounds 7 to 9 (roles switched)

side 4: decisive rounds 10 to 12 (roles switched)

After decisive rounds 3, 6, and 9, roles switch for the next side.
After decisive round 12, the match normally ends.

Starting side (V1)
The initial Emperor player is chosen deterministically from the seed.
This keeps the match reproducible.

Win condition (V1)
Each decisive round gives 1 point to the winner.

After 12 decisive rounds:

if one player has more points, that player wins

if the score is tied 6 to 6, play one extra decisive round

Tie break:

play exactly one extra decisive round with fresh full hands

roles do not switch before or after the tie break

the winner of the tie break wins the match

Engine constraints (V1)
The engine must enforce:

you cannot pick outside of the picking phase

you cannot pick twice before a reveal

you cannot pick a card you do not have in the current round hand

the opponent pick is never visible before reveal