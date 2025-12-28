# Prompt Injection - Sched-yule conflict

**Category:AI**  

# Description

Sir BreachBlocker III has corrupted the Christmas Calendar AI agent in Wareville. Instead of showing the Christmas event, the calendar shows Easter, confusing the people in Wareville.
It seems that without McSkidy, the only way to restore order is to reset the calendar to its original Christmas state. But the AI agent is locked down with developer tokens.
To help Weareville, you must counterattack and exploit the agent to reset the calendar back to Christmas.

---

## Learning objectives
- Understand how agentic AI works
- Recognize security risks from agent tools
- Exploit an AI agent


## Working strat
1. Normal requests to change the event of 25th Dec do not work and we get B64 encoded string 
2. Putting it in cyberchef we get a .json response
3. The thinking section of the chatbot tells us about the presence of the various functions available and tokens available  
4. Getting available functions via prompt
5. Getting token for reset_holiday() via the get_logs() function
6. Using the reset_holiday() func with the required token "and all set!" <just another random ref that popped in my mind for no reason. maybe catching these references is too EZ for u?>

---
## Conclusion
```
flag: THM{XMAS_IS_COMING_BACK}
```
