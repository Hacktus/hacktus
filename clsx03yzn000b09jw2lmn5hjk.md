---
title: "🎮 Diving Back into Games-related Bugs!"
datePublished: Thu Feb 22 2024 09:08:40 GMT+0000 (Coordinated Universal Time)
cuid: clsx03yzn000b09jw2lmn5hjk
slug: diving-back-into-games-related-bugs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708592822819/93c1c525-cde2-4df2-a51c-59b539410edf.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1708592911384/e38c13d3-842b-4718-9203-5e78182b6109.jpeg

---

1. Daily Rewards? 🗓️
    

Although we talked about it last time, this specific one can have a lot of attack vectors. Ever wondered if you could trick a game into giving you daily rewards early? Turns out, you often can. It's as simple as playing around with dates and times. For example, say the game's waiting for it to be 18/11/2023 to dish out the next reward. Just tweak your request from 17/11/2023 12:55:55 to 18/11/2023 12:55:55, or mess with those long tick numbers like changing 638358225550000000 to 638359089550000000 and you might just fool the game into thinking it's reward time. And get this – sometimes, just fast-forwarding your phone's clock does the trick. Simple, but it works still somehow 🤔 cause sometimes the game relies on the client side for such actions as of time calculation.

* [datetimetoticks-converter.com](http://datetimetoticks-converter.com)
    

1. Bypassing the limit of limited offers. 🎁
    

Limited offer items in games are like those exclusive gear everyone wants. But what if you could keep buying them, even after they're supposed to be sold out? I've seen games where resending the purchase request lets you snag the same item over and over. The UI says it's gone, but the backend begs to differ. And for an extra twist, race conditions can make this even more fun. Send a bunch of requests at once, and you might just multiply your loot.

1. Invisible Items for Sale? Yes, Please! 👓
    

Picture this: There's a legendary 'wings' item that's usually off-limits until you hit level 100. But who wants to wait for that? I've found games where swapping the item ID in a purchase request can turn buying a mundane 'rock' into snagging those epic wings. The server sometimes forgets to ask, "Hey, should this player even see this item, let alone buy it?"

1. Equipping items or gear. 🛡️
    

Similar to our shopping attack surface, equipping items can be a goldmine. Ever dreamt of wearing high-level gear while you're still a newbie? Just swap the item IDs in your equip request. Change the ID from a low-level hat to a high-end armor, refresh, and you might just be strutting around in gear way above your pay grade. Sometimes that won't easily work out and can be a bit tricky, so you can mix it with our shop bug, buy a hidden item from shop that you haven't unlocked yet which in our case is the level 100 gear, then try to equip it now after you already bought it although you don't meet the level requirements!

1. Currencies adjustment. 💰
    

In some games, the shop plays it cool with currency options, only showing a few. But what if you change the currency in your purchase request? Try switching from USD to, say, TL, and check out the price difference. Sometimes, the game gets confused and gives you a sweet discount.

1. Skipping the Upgrade Grind ⚙️
    

Upgrading stuff in games can feel like climbing a mountain – it takes time and resources. But here's a neat trick: capture the request for a final upgrade, then use it on a new item or building. Why bother with all the early, expensive steps when you can jump straight to the top?

It's all about getting creative and thinking outside the box. Happy hunting, and stay tuned for more tales from the game-hacking front lines! 🌟🎮