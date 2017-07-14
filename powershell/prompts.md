
Prompt 1: caption, message, choices and default
---


```
$caption = "Choose Action";
$message = "What do you want to do?";
$restart = new-Object System.Management.Automation.Host.ChoiceDescription "&Restart","Restart";
$shutdown = new-Object System.Management.Automation.Host.ChoiceDescription "&Shutdown","Shutdown";
$choices = [System.Management.Automation.Host.ChoiceDescription[]]($restart,$shutdown);
$answer = $host.ui.PromptForChoice($caption,$message,$choices,0)

switch ($answer){
    0 {"You entered restart"; break}
    1 {"You entered shutdown"; break}
}

```

output: 

```
Choose Action
What do you want to do?
[R] Restart  [S] Shutdown  [?] Help (default is "R"):

```
