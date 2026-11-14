# Snake

A responsive Snake game with solo play and real-time lobbies for up to 8 players. A game master hosts each round and can change the arena size or drop an explosion that cuts snakes it hits.

## Play solo

Open `index.html` in a browser, or serve the folder with a static HTTP server. Use the arrow keys or **W A S D** to steer. Press **Space** to pause solo play. On touch screens, swipe on the board or use the direction buttons. Your best solo score is saved in the browser.

## Multiplayer setup

The site is static, so multiplayer uses Supabase Realtime Broadcast. Each browser joins a public room channel named with the lobby code. The code is the room's join pass: share it only with people you want in the game. The browser-safe Supabase publishable key is visible in the public site by design; never use a Supabase secret key here.

1. Create a Supabase project on the [Supabase Dashboard](https://supabase.com/dashboard).
2. In the project, open **Connect** and copy the **Project URL** and **Publishable key**. Use the publishable key (or legacy `anon` key), never a secret or `service_role` key. Supabase documents these as the keys for browser code: [API keys](https://supabase.com/docs/guides/getting-started/api-keys).
3. Put both values in `config.js`:

   ```js
   window.SNAKE_SUPABASE = {
     url: "https://your-project.supabase.co",
     publishableKey: "sb_publishable_..."
   };
   ```

4. In Supabase **Realtime Settings**, keep **Realtime** enabled and **Allow public access to channels** enabled. Public access is enabled by default on new projects. This game uses public channels so players can join with a code without creating accounts. Supabase explains the effect of that setting in its [Realtime settings documentation](https://supabase.com/docs/guides/realtime/settings).
5. Publish the updated files on your web host. On this GitHub Pages repo, merging the multiplayer branch to `main` publishes it after the Pages build completes.

The game master chooses **Create lobby** and shares the generated 8-character code. Other players enter the code and choose **Join lobby**. The host counts toward the 8-player limit and starts the round. During a round, the host can set an arena margin (0–8 cells) and drop an explosion. An explosion warns for a moment, then removes the snake segment it hits and the rest of its tail. A snake is eliminated when it is shorter than 3 segments.

## Run locally

```sh
python -m http.server 8000
```

Then visit <http://localhost:8000>. Multiplayer works locally after filling `config.js` with the same Supabase project values.
