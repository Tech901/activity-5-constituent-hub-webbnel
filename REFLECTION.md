---
title: "Activity 5 Reflection - Constituent Services Hub"
type: reflection
version: "1.0.0"
---

# Activity 5 Reflection

Answer each question in 3-5 sentences. Thoughtful, specific responses earn full credit.

## 1. PII Redaction in Practice

What PII categories did the Azure AI Language service detect in the Memphis 311 complaints? Did you notice any false positives (non-PII flagged as sensitive) or false negatives (PII that was missed)? How might PII detection requirements differ between a government agency processing citizen complaints and a commercial customer service system?

There were lots of addresses, which would be expected for this service since we are trying to get the location of where the issue is at. There were some false positives, such as family, city crew, neigbors being redacted. For this use, we wouldn't want to redact location information, as it's critical to know where the issue is.

## 2. Sentiment as a Routing Signal

How could sentiment analysis be used to prioritize Memphis 311 complaints — for example, routing highly negative complaints to senior staff? What are the risks of relying solely on sentiment for prioritization (consider complaints that are neutral in tone but urgent in nature)? How would you combine sentiment with other signals for a more robust routing system?

I am not sure sentiment analysis would be a worthwhile metric to use, as it would be challenging to find positive comments about 311 tickets. A threshold could be used to have human oversight to see what the extremely negative comments were, but I don't think sentiment analysis will provide much useful information, and api calls could be saved for better use of tokens.

## 3. Multilingual Challenges

How accurately did the Language service detect the language of short versus long text samples? What challenges might arise with code-switching (mixing languages in a single message), which is common in multilingual communities? How would you handle a complaint where language detection confidence is low?

The leanguage detection did a great job. If code switching, then the model's confidence in the language would be lower. One way to account for this would be to set a threshold confidence level, and if the text falls below that, then run a translation model to get everything to english.

## 4. CLU vs. Keyword Matching

Compare the CLU model's intent classification with the keyword-based fallback. In what scenarios did CLU perform better, and where did keyword matching suffice? What are the trade-offs of training and maintaining a custom CLU model versus using a simpler rule-based approach for a city's 311 system? *(If CLU was not configured in your environment, discuss how you would expect it to differ based on the training data in `data/intent_examples.json`.)*

As long as the user used the keywords in the fallback function, it would suffice to accurately predict the intent. However, if they used synonyms or different phrasing, the fallback would not work. One way to check for this would be to check confidence levels on the fallbcak function, and if those didn't meet a threshold, then run the model.

## 5. Pipeline Design

Which step in your NLP pipeline was most critical to get right first, and why? If you were deploying this pipeline for production use handling thousands of complaints daily, what monitoring, fallback mechanisms, or error handling would you add? How would you measure pipeline health over time?

I think the most important step would be the translation model. If the text is not english, then all other functions fall apart. Ensuring the model is fed with the best possible data ensures the best results.