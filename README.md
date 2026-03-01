Zawa is a small, local-first E-Card (Kaiji) game.

This repo is mostly a study sandbox. I am using it to practice:

TypeScript and React in a clean setup

modeling game logic as a pure state machine

writing tests for rules and edge cases

keeping UI simple and readable

V1 runs locally. The code is organized so a server version can be added later, but that is not the goal.

What is in here

core/
Pure game logic. Rules live here.

domain/ types

engine/ state transitions (step)

ports/ interfaces for future storage/transport

local/ local adapters used by V1

web/
UI only. It renders state and sends events. No rules.

docs/
Notes for myself: rules, state machine, and small design decisions.

Core idea

State + Event -> State | Error

That is the whole engine.

Run it

Install

npm i

Start

npm run dev

Notes

If you are reading this: expect small refactors. This is a learning project.