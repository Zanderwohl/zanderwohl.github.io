---
title: WASM for Offline-First Collaboration
subtitle: Running the server's reconciliation algorithm inside every browser.
description: >-
  Theatres have thick walls and bad WiFi. Compiling the reconciliation algorithm
  to WebAssembly let every client run an exact copy of the server's logic, so
  documents survive dropped connections — and one very bad bug.
---
My customers on [Thank You, Five!](https://thankyoufive.live) kept losing WiFi in the middle of rehearsals, and the software has realtime collaboration. Keeping both offline capability and realtime collaboration is a contradiction.

Realtime collaboration requires a constant connection. Spotty WiFi means queuing actions up and sending them later. I am building stage management software for theatre teams, and theatres, with old buildings, thick walls, half of them working out of a basement, are close to the worst case for this. Attenuation is bad through cinderblocks.

WASM fit the problem perfectly. You can compile the same code for your server's architecture and the VM that runs in the browser, and it works identically. I wrote the reconciliation algorithm in Rust and compiled it for the client, so every client runs an exact copy of the server's algorithm. Actions get a temporary timestamp and edits apply locally. When a client that dropped off comes back, the server rewinds time and runs reconciliation against all client actions. The divergent clocks stop mattering, so long as two people aren't editing the exact same characters. The server reconciles, sends the truth back to every client, and each one re-applies the same history plus whatever it has done since. Server and client settle on the full truth through a short conversation, and the document comes out perfectly intact.

To my own surprise, it rescued a document of mine before it ever rescued a customer's. I was dogfooding. I'd volunteered to take minutes at a theatre board meeting, when a double-escaping bug started dropping escaped escape sequences into the text I was typing. My undo made it worse, because undo applies deltas in reverse by character index and the indices were corrupt. But the history had every delta from the beginning, so I reconstructed the whole document by replaying them in order. The design I built for bad WiFi turned out to be the thing that made a bad bug recoverable. The bug has since been fixed, but I'm glad I went the way I did.

It's fast, too. WASM runs slower than native, but considerably faster than JavaScript. It has picked up real speedups over the last few Chrome releases, and new proposals for streaming the bytecode and better dynamic linking are quickly eliminating its downsides. The era of WASM is genuinely cool.
