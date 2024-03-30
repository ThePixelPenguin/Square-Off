####AKA The Bingo Game####
#This program is used in conjunction with bingosync.com to make a real-life bingo board speedrun game!
#This is intended for play in a walkable populated settlement, and all players must have Internet-connected mobile devices. 
#Full rules can be found here: https://docs.google.com/document/d/1uKTJbRp8W9Tf3kbdvQgDlT9pDRzDaCNnuTFc2Y-V8R8/edit?usp=drivesdk
#The rest is up to you! Feel free to make your own goals. Detail on their formatting is found in doc.txt
#If you spot any bugs, you can contact me on Tumblr! @thepixelpenguin

import random

def board(allgoals):
  #SETUP
  nobans = bool(input("Exclude bans? "))
  ubans = bool(input("Any limitations? "))
  if ubans:
    ubans = " "
    while not set(ubans).issubset(set("PTRS/G£OMIE|C1?*")):
      ubans = input(
        """Choose bans by entering their symbols in one unbroken string (e.g. P£T):
Don't use public transport (P)
Don't talk to strangers (T)
Don't run (R)
Don't sit down (S)
Don't use stairs (/)
Don't leave the ground (G)
Don't spend any money (£)
Don't take any photos (O)
Don't use any maps (M)
Don't go indoors (I)
Don't eat or drink anything (E)
Don't touch any walls (|)
Exclude any goals involving colours (C)
Exclude any goals involving other players (1)
Exclude any goals requiring other goals (?)
All of the above (*)
(Enter nothing to cancel)
""").upper()
    if ubans == "*":
      ubans = "PTRS/G£OMIEC1?"
    ubans="".join(set(ubans))
  else:
    ubans=""
  #LOAD GOALS
  print("Choosing goals...")
  codegoals = {}
  donts = 0
  meta = 0
  code = []
  while len(codegoals) < 25:
    r = random.randrange(len(all))
    goal = all[r]
    if not (str(r+1),goal) in codegoals and not (nobans and "Don't" in goal):  #No repeats or bans if disallowed
      types=[]
      if ubans!="" and "(" in goal:
        types = goal[goal.find("("):].strip("()").split(",")
      if types==[] or ubans=="" or set(types).intersection(set(ubans))==set():  #Not universally banned
        if "Don't" in goal:  #Is it a ban?
          if donts < 2: #Only two bans per board
            codegoals[str(r+1)]=goal
            donts += 1
        elif "?" in types: #Is it a meta goal?
          if meta < 1: #Only one meta goal per board
            codegoals[str(r+1)]=goal
            meta += 1
        else:
          codegoals[str(r+1)]=goal
  code = list(codegoals.keys())
  goals = list(codegoals.values())

  #CATEGORISE GOALS
  bans = []
  banned = []
  ends = []
  for goal in goals:
    if "(" in goal:
      if "Don't" in goal:
        bans.append(goal)
      else:
        banned.append(goal)
    if "at the end" in goal:
      ends.append(goal)

  #ARRANGE GOALS
  print("Arranging goals...")
  #No more than one ending per line
  if len(ends) > 1:
    swap = True
    while swap:
      swap = False
      for i in ends:
        for j in ends:
          if i != j:
            a = goals.index(i)
            b = goals.index(j)
            if a // 5 == b // 5:  #Share a row
              goals[b], goals[(b + 5) % 25] = goals[(b + 5) % 25], goals[b]
              swap = True
            elif a % 5 == b % 5 or a % 6 == b % 6 == 0 or a % 4 == b % 4 == 0:  #Share a column or diag
              goals[b], goals[(b + 1) % 25] = goals[(b + 1) % 25], goals[b]
              swap = True

  if not nobans:
    #No self-contradictory lines
    swap = True
    while swap:
      swap = False
      count = 0
      for i in banned:
        types = i[i.find("("):].strip("()").split(",")
        for j in types:
          for k in bans:
            if j in k:  #If goal is banned, move the ban
              a = goals.index(i)
              b = goals.index(k)
              if a // 5 == b // 5:  #Share a row
                if count == 5:  #To avoid infinite loops
                  goals.shuffle()
                  count = 0
                else:
                  goals[b], goals[(b + 5) % 25] = goals[(b + 5) % 25], goals[b]
                  swap = True
                  count += 1
              elif a % 5 == b % 5 or a % 6 == b % 6 == 0 or a % 4 == b % 4 == 0:  #Share a column or diag
                if count == 5:  #To avoid infinite loops
                  goals.shuffle()
                  count = 0
                else:
                  goals[b], goals[(b + 1) % 25] = goals[(b + 1) % 25], goals[b]
                  swap = True
                  count += 1

  #OUTPUT GOALS
  rawgoals = goals[:]
  print("Formatting goals...")
  goals,code = write(goals,code)
  code.append(str(int(nobans))+str(ubans))
  with open("code.txt", "w") as txt:
    print("-".join(code), end="", file=txt)
    txt.close()
  with open("board.txt", "w") as txt:
    print(" \n" * 30, file=txt)
    print("[", end="", file=txt)
    for i in range(24):
      print('{"name": "' + goals[i] + '"},', file=txt)
    print('{"name": "' + goals[24] + '"}]', file=txt)
    print(" \n" * 30, file=txt)
    txt.close()
  print("All done! Board JSON written to board.txt, code written to code.txt")
  return rawgoals, nobans, ubans


def write(goals, code=[], importing=False):
  for i in range(len(goals)):
    goal = goals[i]
    if "(" in goal:
      goal = goal[:goal.find("(")]
    goals[i] = goal.strip()
    if not importing:
      while "[" in goal:
        if goal[goal.find("[") + 1].isdigit():
          x = random.randint(int(goal[goal.find("[") + 1:goal.find("-")]), int(goal[goal.find("-") + 1:goal.find("]")])) #Pick a random number in the range
          if x == 1:
            goal = goal.strip().rstrip("s")
          goal = goal[:goal.find("[")] + str(x) + goal[goal.find("]") + 1:] #Insert x
        else:
          x = chr(random.randint(ord(goal[goal.find("[") + 1]), ord(goal[goal.find("-") + 1]))) #Pick a random letter in the range
          goal = goal[:goal.find("[")] + x + goal[goal.find("]") + 1:] #Insert x
        if code!=[]:
          code[i]+="."+str(x)
    else:
      codei=code[i].split(".")[1:]
      while "[" in goal:
        x = codei[0] #No need for randomness here!
        if x == "1":
          goal = goal.rstrip("s ")
        goal = goal[:goal.find("[")] + str(x) + goal[goal.find("]") + 1:]
        codei=codei[1:]
    goals[i] = goal.strip()
  if code!=[]:
    return goals, code
  else:
    return goals


def inboard(all,code):
  goals=[]
  code=code.split("-")
  for i in code[:25]:
    goals.append(all[int(i.split(".")[0])-1])
  rawgoals = goals[:]
  goals,code = write(goals,code,True)
  with open("code.txt", "w") as txt:
    print("-".join(code), end="", file=txt)
    txt.close()
  with open("board.txt", "w") as txt:
    print(" \n" * 30, file=txt)
    print("[", end="", file=txt)
    for i in range(24):
      print('{"name": "' + goals[i] + '"},', file=txt)
    print('{"name": "' + goals[24] + '"}]', file=txt)
    print(" \n" * 30, file=txt)
    txt.close()
  nobans = bool(code[25][0])
  ubans = str(code[25][1:])
  print("Success!")
  return rawgoals, nobans, ubans


def rule(full):
  #LOAD RULES
  rules = full[:full.index("...")]
  wins = full[full.index("...") + 1:]
  ruleset = []
  covered = []
  name = []

  #ADD RULES
  while rules != [] and bool(input("Add a rule?")):
    newrule = random.choice(rules)
    print(newrule[:newrule.index(".") + 1])
    ruleset.append(newrule)
    rules.remove(newrule)
    name.append(newrule[newrule.index('"')+1:-1])
    if "/" in newrule:
      type = newrule[newrule.index("/") + 1]
      for i in range(len(rules)):
        if "/" + type in rules[i] or "-" + type in rules[i]:
          if "=" in rules[i]:
            type2 = rules[i][rules[i].index("=") + 1]
            for j in range(len(rules)):
              if "-" + type2 in rules[j]:
                rules[j] = ""
          rules[i] = ""
    if "-" in newrule:
      type = newrule[newrule.index("-") + 1]
      if not type in covered:
        for i in range(len(rules)):
          if "=" + type in rules[i]:
            ruleset.append(rules[i])
            print(rules[i][:rules[i].index(".") + 1])
            rules[i] = ""
            covered.append(type)
      else:
        for i in range(len(ruleset)):
          if "=" + type in ruleset[i]:
            print(ruleset[i][:ruleset[i].index(".") + 1])
            try:
              name.remove(ruleset[i][ruleset[i].index('"')+1:-1])
            except:
              break
      if "=" in newrule:
        covered.append(newrule[newrule.index("=") + 1])
    while "" in rules:
      rules.remove("")

  #CHOOSE WIN
  if bool(input("Randomise win condition?")):
    for i in range(len(wins)):
      if "-" in wins[i] and not wins[i][:wins[i].index("-") + 1] in covered:
        wins[i] = ""
    while "" in wins:
      wins.remove("")
    win = random.choice(wins)
  else:
    win=wins[0]
  if name == []:
    name.append("Standard")
  name.sort()
  name.append(win[win.index('"')+1:-1])

  #GAME SET
  print(*name,end=":\n")
  for rule in ruleset:
    print(rule[:rule.index(".")], end=". ")
  print(win[:win.index("!") + 1])


def wildcard(all, goals, nobans, ubans):
  while True:
    wc = random.choice(all)
    if not any(
      (wc in goals, "Don't" in wc, "at the end" in wc, "the game" in wc)):
      if nobans and ubans=="":
        print(write([wc])[0])
        return
      else:
        banned = []
        types = []
        for i in goals:
          if "Don't" in i and "(" in i:
            banned+=(i[i.find("("):].strip("()"))
        for i in ubans:
          banned.append(i)
        if "(" in wc:
          types = wc[wc.find("("):].strip("()").split(",")
        if banned == [] or not any(i in banned for i in types):
          print(write([wc])[0])
          return


def newban(allbans, ubans, goals):
  allowed=False
  while not allowed:
    ban = random.choice(allbans)
    if not ban in goals:
      if "(" in ban and ubans!="":
        type = ban[ban.find("("):].strip("()")
        if not (type in ubans):
          ban = ban[:ban.find("(")]
          allowed=True
      else:
        allowed=True
  print(write([ban])[0])


####main####
goals = []
allbans = []
nobuys = False
nobans = False
rawgoals=[]

file = open("goals.txt", "r")
all = file.readlines()
for i in range(len(all)):
  all[i] = all[i].rstrip("\n")
  if "Don't" in all[i]:
    allbans.append(all[i])
file.close()

file = open("modes.txt", "r")
full = file.readlines()
for i in range(len(full)):
  full[i] = full[i].rstrip("\n")
file.close()

file = open("code.txt", "r")
code = file.readlines()[0]
if code!="":
  print("Code detected. Try to import it?")
  if bool(input("Type anything to confirm: ")):
    rawgoals,nobans,ubans=inboard(all,code)
file.close()

print("\033C")
print("""Use the following commands. The ones in brackets require a board to be generated or imported first.
r: Generate a ruleset.
b: Generate a board.
i: Import a board from a code.
(w: Draw a wildcard.)
(n: Pick a random ban.)
For all queries, just press enter to say no. 
Typing anything will be counted as a yes.
It's just quicker!""")
i = " "
while True:
  i = input("> ").lower()
  if i == "b":
    rawgoals, nobans, ubans = board(all)
  elif i == "r":
    rule(full)
  elif i == "i":
    code=input("Input code: ")
    while len(code.split("-"))!=26 and code!="":
      print("Invalid code!")
      code=input("Input code: ")
    if code!="":
      rawgoals, nobans, ubans = inboard(all,code)
  elif i == "w" and rawgoals != []:
    wildcard(all, rawgoals, nobans, ubans)
  elif i == "n" and rawgoals != []:
    newban(allbans, ubans, rawgoals)
  else:
    print("\b")
