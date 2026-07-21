Over the weekend i spent some time to build a single page app called SupplyWatch. It is a react app that monitors inventory for products and notifies when inventory reaches certain levels. 

It works by tracking the history of each supply and calculating the burn rate as the history values change. Whenever the supply history changes (when a user uses water or eats food), we modify the supplies history with the time and amount.

The challenging part in this app was the prediction and finding how fast an items inventory is going down. 

## Architecture 
- Why the guard `history.length < 2` exists (what's it protecting against?)
	- When predicting the burn rate if only one person is drinking water for example and only drank water once, that doesnt give us enough information to compare the difference between before and after, so we wait until we have more of a trend.
- Why the guard `current <= criticalLevel` exists (what's it protecting against?) 
	- ???
- How rate is computed — specifically, which history points and why
	- We compute the drop rate by looking at the oldest history point and the newest history point. Each history point has a value and a timestamp.
	- The drop rate is calculated subtracting the oldest and newest value.
	- The time passed is calculated by finding the difference of time between the oldest and newest history point.
	- Then we divide the drop rate by the time passed.
- Why the guard `rate <= 0` exists (two things this could mean — name both)
	- If the rate comes back less than 0 we return null, in that case we dont want to show a prediction.
- The final math: distance / rate, and what units come out
	- The distance divided by the rate returns the milliseconds and we convert that to seconds, minutes, and hours, days in the next step.