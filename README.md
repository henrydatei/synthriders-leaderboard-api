# synthriders-leaderboard-api
Get and send scores from/to the leaderboards of the VR rhythm game Synthriders

This project is WIP but you can have a look in `test.ipynb` to see how to send scores to the leaderboard. Unfortunately, you need to authenticate yourself and Synthriders is using the Steamworks API for that. They use `GetAuthSessionTicket` which produces a ticket which is then used (as `nonce`) to authenticate the user with the Synthriders API.

The general workflow is as follows:
- Get the auth session ticket from Steam.
- Use the ticket to authenticate with the Synthriders API. This returns some cookies and a `key` (labeled as `AUTHENTICATION_KEY`).
- Use the `AUTHENTICATION_KEY` to generate a `SALTED_KEY` with MD5 hashing.
- The highscore is just a JSON dict which is encrypted with the `SALTED_KEY` using AES-CBC encryption and PKCS7 padding. The JSON dict contains the score, the level, the difficulty and some other information.
- Send the encrypted highscore to the Synthriders API with the cookies in the header.

If you look in the Python code, you will see that I used a `OrderedDict` for the highscore. Technically a simple JSON dict should work as well, but I want to check if my implementation yields the same results as the game sent to the Synthriders API. You can find the informations that the game sent to the API behind the corresponding calculations.