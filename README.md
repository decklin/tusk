# tusk

A tiny command line wrapper for creating OAuth2 apps for the
[Mastodon](https://mastodon.social) API and using them with `curl`.

# Installation

1. Install [jq](https://stedolan.github.io/jq/).
2. Put `tusk` somewhere in your PATH: `install tusk /usr/local/bin`

# Setup

You will create multiple files here: one for your app (custom bot
code/script/etc), and one for each Mastodon account that uses that app.

 1. Create an app:

        tusk -A my-app.json -n 'My App'

    The name (`-n`) is required, and will be shown when asking accounts
    for approval. You can optionally specify a website with `-w` or
    change the scope from the default of `read write` with `-s`.

 2. Create an access token:

        tusk -a my-app.json -T token.json

    This will ask you to open a URL in a browser where you're logged in
    as your bot's account. Do that, approve the request, and come back
    with the approval code. You now have a token!

 3. Use the token as your bot:

        tusk -t token.json /accounts/verify_credentials

That's it! To help you remember the setup options: uppercase letters
(`-A`, `-T`) will write files (without asking to overwrite, so be
careful!), and lowercase letters will read those files. It's up to you
where to store the files, but keep in mind that token contents *don't*
say what account they are for or that account's ID, so you may want to
put that info in the filename.

# Examples

Once you're set up with a token, a `tusk` command line is basically like
a `curl` command line, but instead of curl options followed by a URL, it
goes in this order:

1. `tusk` options (to set the Mastodon instance and token)
2. An API resource name (only the part after the base API URI)
3. `curl` options

  * To illustrate, here's how you would change your display name after
    creating a token in `token.json`:

        tusk -t token.json /accounts/update_credentials -X PATCH -F display_name="Today's display name"

    (This is actually why I wrote `tusk`! Instead of a literal value there,
    I'm using my nickname followed by a new randomly-selected emoji each
    day, as a goof on the early Mastodon meme of putting a ✅ emoji after
    your name as a fake "verified" symbol.)

  * Here's how you could list the accounts you're following:

        my_id=$(tusk -t token.json /accounts/verify_credentials | jq .id)
        tusk -t token.json /accounts/"$my_id"/following | jq -r '.[].acct'

    Your user ID doesn't change, so as mentioned above, in real-world
    usage you'd probably want to save it rather than looking it up every time.

# Thanks

Darius Kazemi implemented
[a hybrid web form/curl command method of creating an app and token](https://tinysubversions.com/notes/mastodon-bot/index.html),
which inspired this.

# Contact

If you create a bot using this script, I'd love to hear about it.
I'm `@decklin@mastodon.social` and am going to set up some bots on
`botsin.space` soon.
