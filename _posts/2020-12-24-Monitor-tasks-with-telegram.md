---
title: "Monitor your computer tasks with TelegramBot"
excerpt: >-
  I use Telegram Bot to monitor my Python tasks to free myself from computers and enable me to handle multitasks. I found it a satisfying and easy-to-use method to share.
date: '2020-12-24'
thumb_img_path: "https://db3pap007files.storage.live.com/y4mp-d0zf-DOayA8RKsJHLkV94YlrbEDJCZ1xhOqeJIuZLdf52mQngD7ZUJ1w6fZlIBd4-JtyFxFheqnyIHBJRfLHtqyrOfYCyzsIT2WPf80iG990NkjvWNCWGTEgrprdZ_2nqBI5zBTvdjZHC8zEb4kqdQaABnWaVGH-wTJGxaJ56WZFUFZ_s2gOLFeef5utME?width=1352&height=1070&cropmode=none"
content_img_path: "https://db3pap007files.storage.live.com/y4mcR5YS2Qi7VDC9AKJwhGgyb38GXr7Sn4t6Le5zVJurPMBLg8KolMVyYiLKWdRKVI_6gGdIAO7d31jhyX_rdEpngmKd3j_ZB5_eriXCoIliC2Wtm-rIm589rQLA_macKOV6xsIfwCR-Z3zyi5S5Oh30iLwht8-VJXg5HG_t0KqH5rTeHSe5b_oB8g4Z7YZ86ho?width=1024&height=572&cropmode=none"
layout: post
tags:
  - Technology
---

In the passing year, I gradually got into the area of Machine Learning, Reinforcement Learning (RL) specially. This requires me to do a lot of coding work and the training tasks usually takes a long time. For RL, there are usually many training parameters that affects the result and need to be tuned. Then I have to run programs with different parameters in parallel either on the same computer or on different computers. All these makes the monitoring of the progress and results of the tasks complex and unclear. Furthermore, I don't want to stay with the computers when tasks are running but I still need to know the stepwise results of training. I figured out that it should be a very ideal option by adding some extra codes in my program, so that these tasks can send me the latest progress and results to one terminal, which is my mobile phone.

Then I tried many methods that may work.

- The first one is the **SMS service** provided by **Twilio**. One can have a virtual phone number from the service and then using the API of the virtual number, it is possible to send message with the phone number to one's own phone. This enables me to send message to my phone without the need to installing any extra software on my phone. However, I abandoned this method soon because: (1) It is not free. This is not friendly to me as I have to keep using the function as long as I have to code and I don't want to have any limitation in the information size when I want to send it. (2) It is not able to / not easy to send images rather than pure texts while in my work I have to.

- Then I started to try real-time communication tools which are mostly based on the internet. I first tried **WeChat**, which once offered the API to send message through a bot. But now it's gone. Then I tried **Messenger** (Facebook) and **WhatsApp**, both of them open the API but it is for company users / developers, which seems not very easy to access. Fortunately I then found the best solution that will be introduced in the post, the **Telegram bots**.

Telegram bot is a third-party application running inside Telegram. One can easily create a bot and the use the API of the bot to do a lot of things. While I only use the simplest function: sending message to myself. Now I'll briefly introduce how to create a Bot and how I use the bot to help my research. Hopefully it will be helpful to you.

First you have to install [Telegram](https://telegram.org/) to your phone and create an account. Then, search for 'BotFather' in Contacts and create connection with it. 'BotFather' will be the manager of all your own bots. Then send '/newbot' to BotFather and follow the steps, you will create a bot with your customized name. Most importantly, you'll see the **token** of your new bot. You can send '/mybots' to check all your bots and manage them. Clicking on the '@YourBotName', you can go to the dialogue window with your bot. With the **token**, you can access the API of the bot and send message from it. For this specific case, we only need to use the bot to send message to myself. So we have to know the **chat_id** of my telegram account. To get it, search for a bot called 'userinfobot' and start a dialogue with it. The bot will send your id to you automatically.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="https://db3pap007files.storage.live.com/y4m__vC-gCP1tF18g1o4YjZ3x0iFm571eKuUiy3zxxizkz-2AOGcVYkUVPFxyj8G-UQBDXtgzIuPdPuub9ipSgj3gN7Gfy76_pQBY4jsr3hgDu4zUN9gREjMLyQNGMnLH7ZyhVVbTVqCcqk3CVNPHtaLQyXXPXWUN_y4YriQ5HKnbvBxxAQu5FO9oUrkRH2w8eU?width=1489&height=999&cropmode=none">
    </div>
</div>

Then let's go to python on your computer side. First install the library *telegram* by executing:

```csharp
pip install python-telegram-bot --upgrade
```

Then import the *telegram* library in your Python manuscript and create a bot using your bot token. With the bot and your chat_id, you can now send message or images to yourself following the codes:

```python
import telegram
token = 'YourToken'
chat_id = Yourid
bot = telegram.Bot(token=token)
bot.sendMessage(chat_id=chat_id, text='message_content')
bot.send_photo(chat_id=chat_id, photo=open(imageurl, 'rb'), caption=image_caption)
```

Now, run the Python code and check your Phone! You should have received a message from your own bot with your own greetings! For easy of use, I normally package the above codes as libraries and put them in every of my codes, so I can quickly call them easily when I need.

```python
def send_message(message_content):
    token = 'YourToken'
    chat_id = Yourid
    try:
        bot = telegram.Bot(token=token)
        bot.sendMessage(chat_id=chat_id, text=message_content)
    except Exception as e:
        print(e)

def send_image(imageurl, image_caption):
    token = 'YourToken'
    chat_id = Yourid
    try:
        bot = telegram.Bot(token=token)
        bot.send_photo(chat_id=chat_id, photo=open(imageurl, 'rb'), caption=image_caption)
    except Exception as e:
        print(e)
```

Now if you need to send a text message or an image with its caption, you can just call *send_message* or *send_image*. Here are some screenshots of how I am informed of my Python program progresses.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="https://db3pap007files.storage.live.com/y4mgpWCv6x9GG2MIU7S1YhNd68736nteknmtO0MBCPL5TC1LWVtxjoKK5MNH_ZrN32E-LEgHDj2sGwZIW5EVZMNCZMRMv2vFX18-TPYUM9gDXrExUTkCrJtaOncNNxOKVQ2fyZ5gQK59PCYspAMN0XR2XFkyE2fDa4YMea_KY8njgzvd3d5ajSCd4i-21PlaJ7E?width=1702&height=1730&cropmode=none">
    </div>
</div>

I wish this post will be helpful to your own work and bring you some inspirations, and I wish everyone a joyful Christmas eve!