---
id: introduction
title: NextJS App
sidebar_label: NextJS App
---

Symbl is a comprehensive suite of APIs for analyzing natural human conversations - both for your team’s internal conversations and of course the conversations you are having with your customers. Built on our Contextual Conversation Intelligence (C2I) technology, the APIs enable you to rapidly incorporate human-level understanding that goes beyond simple natural language processing of voice and text conversations.

# Getting started

To get started, you’ll need your account credentials and Node.js installed (> v8.x) on your machine.

Your credentials include your appId and appSecret. You can find them on the home page of the platform.

![App ID](https://docs.symbl.ai/images/credentials-faf6f434.png)

After you retrieve them, you need to set them up under `.env` file in this [repo](https://github.com/symblai/nextjs-symblai-demo)

```
APP_ID=
APP_SECRET=
```

## About this app.

This app is using [ReactJS](https://reactjs.org/), [Typescript](https://www.typescriptlang.org/) and [NextJS](https://nextjs.org/). Let's take a look at folder structure

- `pages` In Next.js, a page is a React Component exported from a `.js`, `.jsx`, `.ts`, or `.tsx` file in the pages directory. Each page is associated with a route based on its file name. You can read more about how NextJS works with pages [here](https://nextjs.org/docs/basic-features/pages)

- `pages/api` here reside NextJS API routes. Here we have `call` and `subscribeToPhoneEvents` routes that will be translated to `/api/call` and `/api/subscribeToPhoneEvents` paths in the browser. In this routes we showcase how you can use Symbl Node SDK to get real-time data from API. You can read more about NodeSDK [here](https://docs.symbl.ai/#symbl-sdk-node-js) or read a how to guide [here](https://docs.symbl.ai/#get-live-transcription-phone-call-node-js-telephony)

### Index page

On the first page of the app we showcase how you can call to your phone number (or meeting number) by providing the phone and multiple parameters. You will get conversational insights, transcriptions, messages, which `Symbl.ai` gets from this call. Architecture looks similar to this:

![](https://docs.symbl.ai/images/tutorial_phone_integration-f54ba415.png)

### REST API

Symbl has REST API to do majority of things like getting insights, processing audio, video and text, get conversation data and much more. Rest of the pages of the app are focused on this API and showcase different use cases of Symbl from getting tasks and questions from existing conversation to uploading and processing video and audio files. You can even get transcriptions from the video and by clicking on these transcriptions, navigate to specific parts of the video. You can see this behavior on `/video` page.

# Running app locally

add credentials to `next-config.js` file filling in `APP_ID` and `APP_SECRET` variables.

```javascript
module.exports = {
  env: {
    APP_ID: '',
    APP_SECRET: '',
  },
}
```

run `yarn` or `npm install`. To run the app, use `yarn dev`

Relevant docs section:

- [Getting started with Symbl](https://docs.symbl.ai/#getting-started)
- [API overview using Postman](https://docs.symbl.ai/#postman)
- [Authentication](https://docs.symbl.ai/#authentication)

How Tos are available [here](https://docs.symbl.ai/#how-tos)

In this app represented are the following

- Symbl Node SDK (Check out `api/call` file)
- REST Telephony API (`/phone` page)
- Conversational API (`/conversations` page)
- Async Audio API (`/audio` page)
- Async Video API (`/video` page)
- Async Text API (`/text` page) - Comming soon
- Symbl react elements package available [here](https://www.npmjs.com/package/@symblai/react-elements)
