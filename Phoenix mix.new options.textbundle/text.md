# Phoenix `mix.new` options

I was looking at what `mix phx.new` does in its setup process for my new project. I was unsure how easy it’d be to start without Ecto and HTML since it’s mostly an API to work with (and I’d like to think about how it’ll be possible to add in the future).

So…..how does one figure that out? I don’t want to know what specific lines in files change, but here’s how I’d do that if I wanted to:
1. Create and initialize a git repo: `git init`
2. Run `mix phx.new something --no-ecto --no-html`
3. Commit
4. `rm -rf something && mix phx.new something`
5. Do a `git dfif`

But for just the files added, you can run those two commands, and diffing those two gives us the files that changed:  
 ![](assets/DraggedImage.png)

So it looks like it’s a bunch of view files, the `PageController` (and associated views/templates), `repo` and `migrations` folders.


Strangely enough the assets remain in both runs 🤔 (This is probably for stuff like the `/dashboard` routes