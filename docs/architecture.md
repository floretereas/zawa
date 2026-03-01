Architecture notes

core/
Pure logic. No UI. No network.

domain: types and data shapes

engine: the step function (rules live here)

ports: interfaces for future storage/transport

local: a local runner that connects UI to the engine in V1

web/
UI only.

renders snapshots

sends events

does not contain game rules

docs/
Notes used as the source of truth.

rules.md: rules and constraints

state-machine.md: states, events, and what is allowed

Match shape (V1)
The match has 12 decisive rounds total, split into 4 sides.
Roles switch after the 3rd, 6th, and 9th decisive round.
If the score is tied after 12 decisive rounds, play one extra decisive round.