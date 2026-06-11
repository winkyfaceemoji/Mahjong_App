# Mahjong_App

Multiplayer Hong Kong–style Mahjong for the browser — play with friends, fill empty seats with bots. Plain HTML/JS frontend, Node.js + Socket.io backend.

## Requirements

- [Node.js](https://nodejs.org/) 18+ (includes npm)

## Setup

Install dependencies once:

```sh
npm install
```

## Run

```sh
npm start
```

Then open <http://localhost:3000> in your browser.

1. Enter your name and click **Create Room**.
2. Share the 4-letter room code with friends, or click **🤖 Add Bot** to fill seats (host only).
3. Click **Start Game** (2–4 players, any mix of humans and bots).

### Playing with friends on your network

Friends on the same Wi-Fi/LAN can join at `http://<your-LAN-IP>:3000` — find your IP with:

```sh
ipconfig
```

(use the IPv4 address, e.g. `http://192.168.1.42:3000`), then they enter the room code.

To play over the internet, either forward port 3000 on your router or use a tunnel such as [ngrok](https://ngrok.com/):

```sh
ngrok http 3000
```

### Options (environment variables)

| Variable       | Default | Effect                                  |
| -------------- | ------- | --------------------------------------- |
| `PORT`         | `3000`  | HTTP/WebSocket port                     |
| `BOT_DELAY_MS` | `700`   | Bot "thinking" time per move (ms)       |

PowerShell example:

```powershell
$env:PORT='4000'; $env:BOT_DELAY_MS='300'; npm start
```

## Development

Auto-restart the server on file changes:

```sh
npm run dev
```

## Test

Runs the end-to-end suite (spawns its own server on port 3100, plays a full game against 3 bots, exercises rejoin/reconnect):

```sh
npm test
```

## Project layout

| Path                | Purpose                                              |
| ------------------- | ---------------------------------------------------- |
| `server.js`         | Express + Socket.io server: rooms, game flow, bots   |
| `mahjong.js`        | Core rules: deck, win detection, claim validation    |
| `bot.js`            | Bot AI: discard scoring and claim decisions          |
| `public/index.html` | Lobby: create/join room, add/remove bots             |
| `public/game.html`  | Game board page                                      |
| `public/game.js`    | Client game logic and rendering                      |
| `public/style.css`  | Green-felt theme and tile styles                     |
| `test/e2e.js`       | Self-contained end-to-end test                       |

## Notes

- Refreshing mid-game is safe — your seat is restored automatically (token-based rejoin).
- Rooms live in server memory; restarting the server clears all rooms.
