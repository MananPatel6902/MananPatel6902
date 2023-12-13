### Hi there 👋

<!--
**MananPatel6902/MananPatel6902** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->


from datetime import date, timedelta
from fileinput import input as finput
from math import log

try:
    dates = sorted(date.fromtimestamp(int(line)) for line in finput())
    first, last = dates[0], dates[-1]
except:
    raise SystemExit("Empty or incorrect input!")

data = {last - timedelta(days=x): 0 for x in range((last - first).days + 1)}
for day in dates:
    data[day] += 1

weekdays = {(i - 1) % 7: [] for i in range(7)}
for day in data.keys():
    weekdays[day.weekday()].append(day)

# ansi escape codes to set background to several shades of green
# range(236, 255, 2) gives a nice grayscale option
colors = [
    "\x1b[48;5;{}m  \x1b[0m".format(color)
    for color in [8, 51, 86, 121, 156, 191, 226, 220, 214, 208, 202]
]

first_wk_day = first.weekday()
indent = 0 if first_wk_day != 6 else 7
for wkday, days in weekdays.items():
    if indent <= first_wk_day:
        print("  ", end="")
        indent += 1
    for day in sorted(days):
        heat = min(int(log(data[day], 2) if data[day] else -1) + 1, 10)
        print(colors[heat], end="")
    print()
