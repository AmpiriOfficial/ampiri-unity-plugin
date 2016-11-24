# Guia de Integração para Ampiri Unity Plugin

Conteúdo
=================

* [Começando](#começando)
* [Integrando Banner](#integrando-banners)
* [Integrando Interstitials](#integrando-interstitials)
* [Integrando Video](#integrando-vídeo)
* [Integrando Third Party SDKs](#integrando-sdks-de-terceiros)
* [Passos específicos da plataforma](#passos-específicos-da-plataforma)

Suporte Ampiri
==============

* Nota: Os tutorias a seguir estão em Inglês. A versão em Português estará disponível em breve.

Documentos adicionais sobre a integração do Ampiri SDK com o seu iOS e Android app pode ser encontrado clicando nos links.
- [Tutoriais Ampiri.com](http://www.ampiri.com/tutorials/) - Tutoriais da Ampiri
- [Guia do desenvolvedor](https://ampiri.zendesk.com/hc/en-us/articles/213857245-Publisher-s-Self-Serve-UI-User-Guide) - Guia do desenvolvedor
- Em breve documentações adicionais!

Começando
=================
1. Para ver o exemplo, importe AmpiriUnityDemo.package. Lá você vai ver a scene __DemoApp__ na pasta scenes e dentro da pasta __Scripts__ tem alguns exemplos de uso dos scripts da Ampiri.
2. Certifique-se de que todos os arquivos foram selecionados e clique em __Import__
3. Em __Assets > Ampiri > Prefabs__, arraste Ampiri.prefabs dentro da sua scene.
4. Android/iOS Ad Unit IDs devem ser preenchidos nos campos que aparecerem. Ad Unit IDs estão disponíveis na área de administração do site Ampiri.

Para ver um app de exemplo e o código, entre na pasta __DemoScene__. Lá, você deve ver o arquivo __New_Demo__ e dentro da pasta __Scripts__ estão exemplos de uso dos scripts da Ampiri.

Inicializando
===================
Você deve chamar somente isso **uma única vez** durante o tempo de vida da aplicação:
```
ampiriSDK.sdk.Init("https://api.ampiri.com", true);
```

Integrando Banners
===================
Anúncios no formate de banners são mostrados somente na parte de cima ou de baixo da tela do app.
Para mostrar banner:

Tamanho de banners disponíveis:
* 320x50 (recomendado)
* 728x90

Alinhamentos disponíveis:
* Center
* Left
* Right

Propaganda de Auto Atualização:
* True
* False

```
ampiriSDK.sdk.ShowTopBanner(tamanho, alinhamento, true);
ampiriSDK.sdk.ShowBottomBanner(tamanho, alinhamento, true);
```

Para esconder banner(s):

```
ampiriSDK.sdk.HideTopBanner();
ampiriSDK.sdk.HideBottomBanner();
```

# Banner Callback
```
using UnityEngine;
using UnityEngine.UI;
using System.Collections;

/// <summary>
/// User Example Class on Banners
/// </summary>
public class BannerCallBack : MonoBehaviour {

    [SerializeField]
    Text log;

    void Log(string Text)
    {
        log.text = Text;
    }

    #region Mono
    void OnEnable()
    {
        // Add Standard Banner CallBacks
        AmpiriEvents.Banner_OnLoaded += OnBannerLoaded;
        AmpiriEvents.Banner_OnLoadError += OnBannerLoadError;
        AmpiriEvents.Banner_OnClick += OnBannerClicked;
        AmpiriEvents.Banner_OnNoAd += OnBannerNoAd;
    }
    void OnDisable()
    {
        // Remove Standard Banner CallBacks
        AmpiriEvents.Banner_OnLoaded -= OnBannerLoaded;
        AmpiriEvents.Banner_OnLoadError -= OnBannerLoadError;
        AmpiriEvents.Banner_OnClick -= OnBannerClicked;
        AmpiriEvents.Banner_OnNoAd -= OnBannerNoAd;
    }
    #endregion

    #region Banner CallBacks

	// Callback triggers when banner has successfully loaded
    public void OnBannerLoaded()
    {
        Log("Banner_OnLoaded");
    }
	// Callback triggers when banner has been clicked
    void OnBannerClicked()
    {
        Log("Fullscreen_OnBannerClick");
    }
    // Callback triggers when loading of banner returns error
    void OnBannerLoadError()
    {
        Log("Banner_OnLoadError");
    }
    // Callback triggers when loading of banner returns no advertisements
    void OnBannerNoAd()
    {
        Log("Banner_OnNoAd");
    }
    #endregion
}
```

Integrando Interstitials
==========================
O tamanho do banner é definido automaticamente, dependendo do tamanho da tela.

Para *carregar* um banner interstitial:
```
ampiriSDK.sdk.LoadFullscreen();
```
Para *mostrar* um banner interstitial:
```
ampiriSDK.sdk.ShowFullscreen();
```
Para *carregar* e *mostrar* um banner interstitial:
```
ampiriSDK.sdk.LoadAndShowFullscreen();
```
Para *carregar* e *mostrar* um banner interstitial com atraso:
```
ampiriSDK.sdk.LoadAndShowWithDelayFullscreen();
```
# Exemplo de Interstitial Callback
```
using UnityEngine;
using UnityEngine.UI;
using System.Collections;

/// <summary>
/// Interstitial callbacks example
/// </summary>
public class InterstitialCallbacks : MonoBehaviour {

    [SerializeField]
    Text statusText;

    [SerializeField]
    FunctionalTestingExample fntest;

    [SerializeField]
    Button showButton;

    [SerializeField]
    Button loadnshowButton;

    [SerializeField]
    Button loadnshowDelayButton;

    void Log(string text)
    {
        statusText.text = "Current status: " + text;
    }

    void OnEnable()
    {
        AmpiriEvents.Interstitial_OnBannerLoaded += OnFullScreenBannerLoaded;
        AmpiriEvents.Interstitial_OnBannerLoadError += OnFullScreenBannerLoadError;
        AmpiriEvents.Interstitial_OnBannerClick += OnFullScreenBannerClicked;
        AmpiriEvents.Interstitial_OnBannerClose += OnFullScreenBannerClose;
        AmpiriEvents.Interstitial_OnBannerNoAd += OnFullScreenBannerNoAd;
    }

    void OnDisable()
    {

        AmpiriEvents.Interstitial_OnBannerLoaded -= OnFullScreenBannerLoaded;
        AmpiriEvents.Interstitial_OnBannerLoadError -= OnFullScreenBannerLoadError;
        AmpiriEvents.Interstitial_OnBannerClick -= OnFullScreenBannerClicked;
        AmpiriEvents.Interstitial_OnBannerClose -= OnFullScreenBannerClose;
        AmpiriEvents.Interstitial_OnBannerNoAd -= OnFullScreenBannerNoAd;

    }

	// Callback triggers when Interstitial has successfully loaded
    void OnFullScreenBannerLoaded()
    {
        Log("FullScreen_OnBannerLoaded");
        showButton.interactable = true;
        loadnshowButton.interactable = loadnshowDelayButton.interactable = false;
    }
	// Callback triggers when Interstitial has closed
    void OnFullScreenBannerClose()
    {
        Log("Fullscreen_OnBannerClose");
        fntest.ExitTransition();
        showButton.interactable = false;
        loadnshowButton.interactable = loadnshowDelayButton.interactable = true;
    }
	// Callback triggers when Interstitial has been clicked
    void OnFullScreenBannerClicked()
    {
        Log("Fullscreen_OnBannerClick");
        fntest.ExitTransition();
        showButton.interactable = false;
        loadnshowButton.interactable = loadnshowDelayButton.interactable = true;
    }
	// Callback triggers when Interstitial returns and error while loading
    void OnFullScreenBannerLoadError()
    {
        Log("Fullscreen_OnBannerLoadError");
        fntest.ExitTransition();
        showButton.interactable = false;
        loadnshowButton.interactable = loadnshowDelayButton.interactable = true;
    }
	// Callback triggers when Interstitial has no advertisments
    void OnFullScreenBannerNoAd()
    {
        Log("Fullscreen_OnBannerNoAd");
        fntest.ExitTransition();
        showButton.interactable = false;
        loadnshowButton.interactable = loadnshowDelayButton.interactable = true;
    }

    void Awake()
    {
        showButton.interactable = false;
    }

}

```

Integrando Vídeo
===================

Para carregar video:
```
ampiriSDK.sdk.LoadVideo();
```

Uma vez que o vídeo foi carregado com sucesso, você pode usar o callback AmpiriEvents.video_OnBannerLoaded para mostrar o vídeo:
```
if (ampiriSDK.sdk.VideoIsReady())
{
    Log("Video_OnBannerLoaded ready YES");
    ampiriSDK.sdk.ShowVideo();
}
else

    Log("Video_OnBannerLoaded ready NO");
}
```

# Exemplo de Video Callback
```
using UnityEngine;
using UnityEngine.UI;
using System.Collections;

/// <summary>
/// Class handles or video callbacks
/// </summary>
public class VideoCallBack : MonoBehaviour {

    [SerializeField]
    AdsSdk ampiriSDK;

    [SerializeField]
    FunctionalTestingExample fntest;

    [SerializeField]
    Text text;

    void OnEnable()
    {
        AmpiriEvents.video_OnBannerLoaded += OnVideoBannerLoaded;
        AmpiriEvents.video_OnBannerOpen += OnVideoBannerOpen;
        AmpiriEvents.video_OnVideoStarted += OnVideoStarted;
        AmpiriEvents.video_OnBannerLoadError += OnVideoBannerLoadError;
        AmpiriEvents.video_OnBannerClose += OnVideoBannerClose;
        AmpiriEvents.video_OnVideoCompleted += OnVideoCompleted;
        AmpiriEvents.video_OnBannerNoAd += OnVideoNoAd;
    }

    void OnDisable()
    {
        AmpiriEvents.video_OnBannerLoaded -= OnVideoBannerLoaded;
        AmpiriEvents.video_OnBannerOpen -= OnVideoBannerOpen;
        AmpiriEvents.video_OnVideoStarted -= OnVideoStarted;
        AmpiriEvents.video_OnBannerLoadError -= OnVideoBannerLoadError;
        AmpiriEvents.video_OnBannerClose -= OnVideoBannerClose;
        AmpiriEvents.video_OnVideoCompleted -= OnVideoCompleted;
        AmpiriEvents.video_OnBannerNoAd -= OnVideoNoAd;
    }


    void Log(string Text)
    {
        if(text != null)
            text.text = Text;
    }

    // Callback triggers when video has loaded
    void OnVideoBannerLoaded()
    {
        if (ampiriSDK.sdk.VideoIsReady())
        {
            Log("Video_OnBannerLoaded ready YES");
            ampiriSDK.sdk.ShowVideo();
        }
        else

            Log("Video_OnBannerLoaded ready NO");
        }
    }
	// Callback triggers when video has opened
    void OnVideoBannerOpen()
    {
        Log("Video_OnBannerOpen");
    }
    // Callback triggers when video closes
    void OnVideoBannerClose()
    {
        Log("Video_OnBannerClose");
    }
	// // Callback triggers when loading of video has returned an error
    void OnVideoBannerLoadError()
    {
        Log("Video_OnBannerLoadError");

        fntest.ExitTransition();
    }
	// // Callback triggers when video has completed
    void OnVideoCompleted()
    {
        Log("Video_OnVideoCompleted");
        fntest.ExitTransition();
    }
	// Callback triggers when video has started
    void OnVideoStarted()
    {
        Log("Video_OnVideoStarted");
    }

	// Callback triggers when video has no advertisments
    void OnVideoNoAd()
    {
        Log("Video_OnVideoNoAd");
    }

}
```
Integrando SDKs de terceiros
=============================
#### Android
Para integrar SDK de terceiros no Android, por favor siga os passos em [Passos específicos da plataforma](#passos-específicos-da-plataforma) abaixo.

#### iOS
1. Por favor, baixe o pacote Unity da pasta "Third Party SDKs".
2. Depois de baixar o pacote, duplo clique em &lt;network&gt;.unitypackage e importe todos os arquivos.


Passos Específicos da Plataforma
=======================

#### Android

* Para rodar um exemplo de Android app, importe o projeto AmpiriUnityPluginTestApp_Android para o Android Studio e execute.
* Para rodar o Unity app, siga as intruções na seção **Compilando e Rodando Android Ampiri SDK**. No ponto 2 arraste as scenes do seu app para a janela __Scenes In Build__.

#### iOS

1. Entre em __File > Build Settings__, arraste e solte as scenes que você quer incluir na build para __Scenes in Build__.
2. Na seção __Platform__ selecione __iOS__. Configure o build a direita da seção __Platform__, e selecione __Build__ no menu.
> **Nota: Você deve ser questionado sobre baixar e instalar o __iOSSupport Installer__.**
3. Selecione a pasta onde você quer compilar seu projeto, clique __Save__.
4. Abra os arquivos exportados no Xcode.

Compilando e Rodando Android Ampiri SDK
=======================================

Para rodar o Ampiri SDK na versão Android do seu Unity app, siga os passos:

1. Clique em __File > Build Settings__.
2. Selecione __Android__ na lista de __Platforms__ e marque o checkbox __Google Android Project__. Arraste todos as scenes exigidas pelo seu app para __Scenes In Build__.
3. Clique em __Player Settings__ e configure o __Bundle Identifier__.
4. Clique no botão __Export__ e escolha onde você quer exportar o projeto.
5. Baixe e instale o [Android Studio](https://developer.android.com/studio/index.html) e configure o ambiente de desenvolvimento Android se você não fez isso ainda.
6. No Android Studio clique em __File > New > Import Project__. Escolha o local do projeto exportado especificado no passo 4.
7. Configure o projeto de acordo com [Ampiri SDK Instructions](https://github.com/ampiri/ampiri-android-sdk).

## Compilando e rodando iOS Ampiri SDK

Para rodar o Ampiri SDK na versão iOS do seu Unity app, siga os passos:

1. Selecione __File > Build Settings__.
2. Selecione __iOS__ na lista __Platforms__. Arraste todos as scenes exigidas para __Scenes In Build__.
3. Selecione __Player Settings__ e configure __Bundle Identifier__.
4. Olhe em __Target minimum iOS Version__ e selecione __8.0__.
5. Clique no botão __Export__ e escolha onde você quer exportar o projeto.
6. Abra o projeto exportado no Xcode.
7. Selecione o projeto e em __General Tab__, configure __signing__.
8. Role pra baixo e olhe em __Embeddded Binaries__, clique no botão __"+"__ e selecione __Plugins > iOS > AMPVastLib.Framework__.
9. Na parte de cime do menu Xcode, selecione __Product > Build__.
