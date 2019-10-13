==========================
Servicios Google
==========================

Consola de google
==========================

- Autorizar la APP 
- Pide la clave SHA1
- En la terminal buscar el fichero .keystore

.. code-block:: PowerShell

	keytool -list -v -keystore googleplay.keystore

- Añadir marcadores
- Obtener recursos
- Copiar el texto xml

Unity
==========================

- Importar el paquete google play services desde GitHub
- Menú Window -> Google Play Games -> Setup
- Pegar ahí el texto xml

LeaderBoardGoogle.cs
==========================

.. code-block:: c#

	using UnityEngine;
	using System.Collections;
	using GooglePlayGames;
	using GooglePlayGames.BasicApi;
	using UnityEngine.SocialPlatforms;

	public class LeaderboardGoogle : MonoBehaviour {

	void Start ()
	{

	PlayGamesClientConfiguration config = new PlayGamesClientConfiguration.Builder()
		// enables saving game progress.
		//.EnableSavedGames()
		// registers a callback to handle game invitations received while the game is not running.
		//.WithInvitationDelegate(<callback method>)
		// registers a callback for turn based match notifications received while the
		// game is not running.
		//.WithMatchDelegate(<callback method>)
		// requests the email address of the player be available.
		// Will bring up a prompt for consent.
		//.RequestEmail()
		// requests a server auth code be generated so it can be passed to an
		//  associated back end server application and exchanged for an OAuth token.
		.RequestServerAuthCode(false)
		// requests an ID token be generated.  This OAuth token can be used to
		//  identify the player to other services such as Firebase.
		.RequestIdToken()
		.Build();

	PlayGamesPlatform.InitializeInstance(config);
	
	// recommended for debugging:
	PlayGamesPlatform.DebugLogEnabled = true;
	
	// Activate the Google Play Games platform
	PlayGamesPlatform.Activate();

	Social.localUser.Authenticate((bool success) => {
		if(success) print("ok");
		else print("no");
	});

	OnAddScoreToLeaderBoard();
	}


.. code-block:: c#

	public void OnShowLeaderBoard ()
	{
		// Show all leaderboard
		//Social.ShowLeaderboardUI (); 
		// Show current (Active) leaderboard
		PlayGamesPlatform.Instance.ShowLeaderboardUI (GPGSIds.leaderboard_atraplas); 
	}


.. code-block:: c#

	public void OnAddScoreToLeaderBoard ()
	{
		if (Social.localUser.authenticated) {
		Social.ReportScore (PlayerPrefs.GetInt ("puntosMax"), GPGSIds.leaderboard_atraplas, (bool success) =>
		{
			if (success) {
				Debug.Log ("Update Score Success");
				}
 
			else {
				Debug.Log ("Update Score Fail");
				}
			});
		}
	}