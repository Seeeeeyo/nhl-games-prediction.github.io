---
layout: post
title: Data Acquisition
---

## Data Acquisition
*Write a brief tutorial on how your team downloaded the dataset.*

We used the NHL public API to scrape data about hockey games from 2016 till 2021 and their events. 


https://statsapi.web.nhl.com/api/v1/{gameID}/{feed}/live

It's pretty easy to collect the data for a particular game through this API. The game ID consists of 10 digits. More information can be found here [here](https://github.com/dword4/nhlapi) 

Here are the libraries that we used:
```python
import requests
import pandas as pd
import json
import os
from tqdm import tqdm
```
We first create an object *data manager*. We then use this object to *load* a particular season. If the *season.csv* is already on our computer, we use this one. Otherwise, we collect the game data of all game id of the season using *load_online* and *load_game*. Again, if we already have the json files locally for a game, we don't download it again. We then create a tidy dataframe for each game for the next parts of this project. Once all the games of a season are saved locally, we create a *season.csv* file.
We first *load* all the games of particular seasons and save them locally as json files. If we already have a particular game on our machine, it will use it instead of downloading it again.
We then select the data we're looking for in our application. We create dataframe of tidy data for each game. We them merge all the games to result into a csv file for each season.

```python
class data_manager():
    def __init__(self, url, path=""):
        """
        :param
        url: The url of the REST API
        path : the path between your code and the data folder
        """

        self.url = url

        self.number_of_match = 0
        self.starting_dict = dict({

                                      "event": [],
                                      "game_time": [],
                                      "period_info": [],
                                      "period": [],
                                      "game_ID": [],
                                      "team_info": [],
                                      "coord_x": [],
                                      "coord_y": [],
                                      "shooter_name": [],
                                      # "rinkSide" : [] ,
                                      "goalie_name": [],
                                      "shot_type": [],
                                      "net_empty": [],
                                      "strength": [],
                                      "season": [],
                                      "game_type": [],
                                      "game_number": []

                                  }.copy())
        self.data = dict(self.starting_dict.copy())
        self.path = path
        self.all_data = dict(self.starting_dict.copy())

    def TidyData(self, dataset):
        """
        :param dataset: The raw dataset as a pandas dataframe converted directly from the raw json file
        :return: nothing. The function will store the processed data in self.data
        """

        gametype = {
            "01": "preseason",
            "02": "regular",
            "03": "playoff",
            "04": "all-star"
        }
        teams = dataset["gameData"]["teams"]
        gameID = dataset["gamePk"]["game"]

        events = ["Goal", "Shot"]
        plays = dataset["liveData"]["plays"]

        away = dataset["gameData"]["teams"]["away"]["name"]
        home = dataset["gameData"]["teams"]["home"]["name"]
        for event in plays["allPlays"]:
            a = event["result"]["event"]
            if a in events:

                self.data["event"].append(a)
                self.data["game_time"].append(event["about"]["periodTime"])
                self.data["period_info"].append(event["about"]["periodType"])
                self.data["game_ID"].append(gameID)
                self.data["team_info"].append(event["team"]["name"])
                self.data["period"].append(event["about"]["period"])

                try:
                    if event["result"]["event"] == "Shot" or event["result"]["event"] == "Goal":
                        x = event["coordinates"]["x"]
                        y = event["coordinates"]["y"]
                        self.data["coord_x"].append(x)
                        self.data["coord_y"].append(y)
                    else:
                        self.data["coord_x"].append(None)
                        self.data["coord_y"].append(None)
                except:
                    self.data["coord_x"].append("N/A")
                    self.data["coord_y"].append("N/A")

                try:
                    if a == "Shot" or a == "Goal":
                        self.data["shot_type"].append(event["result"]["secondaryType"])
                    elif a == "Goal":
                        self.data["shot_type"].append(self.data["shot_type"][len(self.data["shot_type"])])
                    else:
                        self.data["shot_type"].append(None)
                except:
                    self.data["shot_type"].append("N/A")
                self.data["strength"].append(
                    None if event["result"]["event"] == "Shot" else event["result"]["strength"]["name"])

                self.data["shooter_name"].append(event["players"][0]["player"]["fullName"])  # doublecheck if always 0

                goalie = None
                for player in event["players"][1::]:
                    if player["playerType"] == "Goalie":
                        goalie = player["player"]["fullName"]
                self.data["goalie_name"].append(goalie)

                net_empty = None
                if event["result"]["event"] == "Goal":
                    if goalie == None:
                        net_empty = False
                    else:
                        net_empty = True
                self.data["net_empty"].append(net_empty)

                self.data["season"].append(str(gameID)[0:4])
                try:
                    self.data["game_type"].append(gametype[str(gameID)[4:6]])
                except:
                    self.data["game_type"].append(str(gameID)[4:6])

                self.data["game_number"].append(str(gameID)[6::])
        return



    def clear(self):
        """
        This function purge the current dataset stored in order to load a new season without aggregating to the old season's data.
        :return: nothing
        """
        for (k, v) in self.all_data.items():
            self.all_data[k] = []



    def load_online(self, path2, season):
        """
        :param path2: The path where the data folder can be found
        :param season: The current season in which we want the data
        :return:
        """

        schedule_url = f"https://statsapi.web.nhl.com/api/v1/schedule?startDate={season}-10-01&endDate={season + 1}-09-30"
        r = requests.get(schedule_url)
        response = r.status_code

        if response == 200:
            tqdm.write("The server has been accessed succesfully")
        elif response == 404:
            raise Exception("Connection Error 404")
        else:
            raise Exception(f"error {r.status_code}")

        schedule = r.json()

        for date in tqdm(schedule["dates"], mininterval=1):
            for game in date["games"]:
                self.load_game(game["gamePk"], season)

        copy = dict(self.data.copy())
        pd.DataFrame(copy).to_csv(path2, index=False)
        return



    def load(self, season, reload=False):
        """
        :param season: The season from which we want the data
        :param reload: If true, the data will be processed again and the csv file matching this season will be overwritten
        :return: nothing
        """

        path2 = os.path.join(self.path, "data", "processed", f"{season}.csv")

        if not reload:
            if os.path.isfile(path2):
                self.data = pd.read_csv(path2)
            else:
                self.load_online(path2, season)
        else:
            self.load_online(path2, season)

        for k, v in self.data.items():
            self.all_data[k].extend(v)

        if type(self.data) != dict:  # pandas converts it to a dataframe without my permisssion
            self.data = self.data.to_dict()
        for k, v in self.data.items():
            self.data[k] = []



    def load_game(self, game_ID, season):
        """
        :param
        game_ID: The game_ID which identifies a particular game.
        season : the season in which we are interested
        :return: Nothing
        """

        path = os.path.join(self.path, "data", "raw", f"{game_ID}.json")
        if os.path.isfile(path):
            dataset = pd.read_json(path)
            self.TidyData(dataset)

        else:
            # tqdm.write(f"Could not load file locally for game {game_ID}",end = "\r")
            dataset = pd.read_json(self.url + f"{game_ID}/feed/live")
            dataset.to_json(path)
            self.TidyData(dataset)

        return



if __name__ == "__main__":
    dm = data_manager("https://statsapi.web.nhl.com/api/v1/game/",path="")
    dm.load(2016, reload=True)
    dm.load(2017, reload=True)
    dm.load(2018, reload=True)
    dm.load(2019, reload=True)
    dm.load(2020, reload=True) 
```
