==========================
Clasificaciones con Dreamlo
==========================


Añadir puntuación
==========================

	if (score > PlayerPrefs.GetInt("HighScore"))
	{
		PlayerPrefs.SetInt("HighScore", score);
		Highscores.AddNewHighscore(PlayerPrefs.GetString("name"), score);
	}


Highscores.cs
==========================

.. code-block:: c#

	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;

	public class Highscores : MonoBehaviour
	{

		const string privateCode = "xxx";
		const string publicCode = "yyy";
		const string webURL = "http://dreamlo.com/lb/";

		DisplayHighscores highscoreDisplay;
		public Highscore[] highscoresList;
		static Highscores instance;

		void Awake()
		{
			highscoreDisplay = GetComponent<DisplayHighscores>();
			instance = this;
		}

		public static void AddNewHighscore(string username, int score)
		{
			instance.StartCoroutine(instance.UploadNewHighscore(username, score));
		}

		IEnumerator UploadNewHighscore(string username, int score)
		{
			WWW www = new WWW(webURL + privateCode + "/add/" + WWW.EscapeURL(username) + "/" + score);
			yield return www;

			if (string.IsNullOrEmpty(www.error))
			{
				print("Upload Successful");
				DownloadHighscores();
			}
			else
			{
				print("Error uploading: " + www.error);
			}
		}

		public void DownloadHighscores()
		{
			StartCoroutine("DownloadHighscoresFromDatabase");
		}

		IEnumerator DownloadHighscoresFromDatabase()
		{
			WWW www = new WWW(webURL + publicCode + "/pipe/");
			yield return www;

			if (string.IsNullOrEmpty(www.error))
			{
				FormatHighscores(www.text);
				highscoreDisplay.OnHighscoresDownloaded(highscoresList);
			}
			else
			{
				print("Error Downloading: " + www.error);
			}
		}

		void FormatHighscores(string textStream)
		{
			string[] entries = textStream.Split(new char[] { '\n' }, System.StringSplitOptions.RemoveEmptyEntries);
			highscoresList = new Highscore[entries.Length];

			for (int i = 0; i < entries.Length; i++)
			{
				string[] entryInfo = entries[i].Split(new char[] { '|' });
				string username = entryInfo[0];
				int score = int.Parse(entryInfo[1]);
				highscoresList[i] = new Highscore(username, score);
				print(highscoresList[i].username + ": " + highscoresList[i].score);
			}
		}

	}

	public struct Highscore
	{
		public string username;
		public int score;

		public Highscore(string _username, int _score)
		{
			username = _username;
			score = _score;
		}

	}


DisplayHighscores.cs
==========================

.. code-block:: c#

	using UnityEngine;
	using System.Collections;
	using UnityEngine.UI;

	public class DisplayHighscores : MonoBehaviour
	{

		public Text[] highscoreFields;
		Highscores highscoresManager;

		void Start()
		{
			for (int i = 0; i < highscoreFields.Length; i++)
			{
				highscoreFields[i].text = i + 1 + ". Fetching...";
			}

			highscoresManager = GetComponent<Highscores>();
			StartCoroutine("RefreshHighscores");
		}

		public void OnHighscoresDownloaded(Highscore[] highscoreList)
		{
			for (int i = 0; i < highscoreFields.Length; i++)
			{
				highscoreFields[i].text = i + 1 + ". ";
				if (i < highscoreList.Length)
				{
					highscoreFields[i].text += highscoreList[i].username + " - " + highscoreList[i].score;
				}
			}
		}

		IEnumerator RefreshHighscores()
		{
			while (true)
			{
				highscoresManager.DownloadHighscores();
				yield return new WaitForSeconds(30);
			}
		}
	}

