# analytics-learn
All about analytics - what to look at, how to get it, and how to transform data into actionable actions. A thorough research on specific user behaviour on app/web or how to instill targetted behaviours.



## Decision
Make purchase decision easier
- create reusable profiles for recurring purchases. User can switch the profile for family, self, etc
- Is it a one time purchase? Make the forms easier to purchase.
- How to convince user that the right decision is being made? Once the user select the item, throw the anchor 80% of the user buy this too (this statistic is based on the what the customers purchase the most in general, we do not include the actual number, just the percentage. Also, we do not show it in the listing page to reduce bias on what products to buy. We just want to trigger the user to make the final decision.
- Instead of showing a listing, do a listing with a recommendation. E.g. how about purchasing xyz. This gives user an option straight away if they do not know what the are looking for.


Create a lead tracker that allows scraping potential leads. Also, use google url tracker to see how is the progress. When providing url to the leads, customise the websites content to fit what the leads are interested in.

## Reviews


What are reviews for.

People who write reviews
- express positive thought
- Express discontent
- Writing for the sake of writing. These are just meant to boost the review with no context whatsoever.


People who read reviews
- ironically, it’s not meant to understand the product. It’s meant to validate. I only want to hear what I only want to hear. 
- They want to relate their experience with those who bought it
- Analysis for product market fit
- Know what your competitor is doing good. Feedback cycle.


What could be done?
Product to product comparison/review.
Stats of growth 
Sentiment to find the top 10 positive/negative reviews.
Mining reviews is hard, what if we can show users the reviews with the most context?  E.g. those with words like use, purchase, customer service, experience would be boosted, as this are the ones that leads to conversions
Call to actions for partners
- embeddable survey or poll
- In the end, what partners want is some value and control over what we have. 
Extract verbs to find out what users do with the product




## Making hypothesis 

Hypothesis: User likes the color blue

How to measure execution time of user to complete an action?
Simple, when the component starts, set the start time. When the user completed the action such as submitting a form, record the time taken.
Catfooding. Use a competitor app and try out their functions. What did they do well, what they did not do well?
Dogfooding. Use our own app. List back our top features (should be measurable), and give a list of tasks. See how long it takes to complete them.

Recording time. Users can just enter and exit the feature. We do not want to record that (sampling). We only sample if the user stays on the screen more than X seconds, or has initiated the first step.

```
Define Decision
Recommend the user … 
Start Date:
End Date:
Difference measured during this period:
Does it work well?
Notes
```

## Useful patterns for data
- Tag the start date. This allows us to know if the report is indeed stale or not. Always strive to produce a better ones.
- add similar tags

Create user profile for each individuals.
- Store the actions/events/behaviours in a bucket for each user
- Whenever a new user action is performed, gather the metrics
- For each metrics, assign a behaviour, e.g. if the user login actively, give them the behaviour “active user”

Upload documents/data
- allow users to ask questions on the documents they uploaded. For example if it is an insurance policy, users can ask if they are covered for certain incident

## Slowly changing dimensions/Trends/Seasonal

How do we deal with such analytics? Simple, add a time decay.

## Learning from Decisions

can we add a page called downtime in Confluence, and record what happened, what we did, how we approach it, what works/what doesn’t, how long was the downtime and when it recover?



## how to track user activity across different devices and time?
we can use bitmap to track user’s daily and hourly records
- weekly: [sun, mon, tue, wed, thu, fri, sat] days
- daily: [0…23] hours
From here we can find the most common day that the user login, and the most frequent time that the user is online. If we gather this enough for a month, we can have the data average presented.
How long should the data be kept? Probably 1 month old, so what we will do is collapse the old data counts 

Find the most visited site for each user. and from there we can infer if the content is what they need.

users_most_visited_pages: []
users_most_online_time: []
users_average_time_on_device: []
users_time_taken_to_complete_process… (can be a good measure for forms etc, to improve the form design etc)
users_most_searched_keyword: []

## how_does_ab_testing work?

## bandit algorithms

## Experiments
- headline testing


## Metrics

What are the core metrics required for the team for a particular field?
- CI/CD
- ecommerce
- marketing
- sql 

## References

https://sparktoro.com/blog/how-to-measure-hard-to-measure-marketing-channels/
