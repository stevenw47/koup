# koup
I created koup because my friends and I wanted to play Coup when we were outside (e.g. at a restaurant), but didn't have any playing cards to use.

Koup is *not* designed to be a method of playing Coup online. Rather, it is an augmentation of real-life play that allows you to use your phone as the "cards". You can then perform the same actions on those cards as you would normally. It also helps you manage your coins (so you don't need to count) and does the shuffling of the deck for you (read: hassle free)!

The backend API can be found [here](https://github.com/lazypanda10117/koup-api) and was developed by [@lazypanda](https://github.com/lazypanda10117).

## Note
Due to limitations with (free) Heroku, the backend will automatically sleep after 30 minutes of inactivity.
Therefore, the first call to the backend (most likely after clicking `Create Game`) may take ~30 sec while the dyno wakes up.

## Todo
- Have proper error handling (currently does an alert)
- Change favicon
- Other things that are marked TODO in the comments

## Setup
```
yarn install   # Project setup
yarn run serve # Compiles and hot-reloads for development
yarn run build # Compiles and minifies for production
yarn run lint  # Lints and fixes files
```
