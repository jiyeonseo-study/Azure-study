# Creating Chatbot with Bot Framework and Bot Service 

## Prerequisites 
- IDE : [Visual Studio 2017](https://visualstudio.microsoft.com/ko/downloads/)
  - Extensions : Bot Builder Template for Visual Studio 
  - 안타깝게도 Mac에서는 안되는 것 같다... 
    - 이러한 오픈 소스도 있음. https://github.com/User1m/botframework-template-vs-for-mac 
- Azure Account


## Web App Bot
- Bot Framework + Azure Storage => Bot Service 를 통틀어.
- 다양한 매신저 Channel들 지원: Skype, Telegram, Slack, Facebook... 

## Steps to build the bot
### 1) ADD Resource Group
- Resource Group 이름은 내 계정 안에서만 Unique 
### 2) 생성한 Resource Group에서 Web App Bot 생성(create)
- Pricing tier
  - F0 : Free
  - S1 : Standard
### 3) Visual Studio 에서 Project 생성
1. extension 을 이용한 Bot builder template 이용 
2. Azure Portal > 봇 > Build > download bot source code 하여 Visual Studio에 import. 

