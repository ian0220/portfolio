
```css

using UnityEngine.UI;
using TMPro;

public class InterFaceManger : MonoBehaviour
{
   
    [SerializeField]
    private GameObject[] m_emotionsButtons;
    [SerializeField]
    private GameObject m_parentButton;
    private List<GameObject> m_ChosenEmotion;
    private WhatEmonion m_whatEmonion;
    public WhatEmonion m_ButtonEmonin;
    private List<int> m_chosenEmotionPostion;
    private List<int> m_chosenEmotionNumber;

    [SerializeField]
    private int m_winPoints;
    [SerializeField]
    private GameObject m_winObject;
    [SerializeField]
    private int m_losingPoints;
    [SerializeField]
    private GameObject m_losingObject;

    [SerializeField]
    private GameObject m_coin;
    [SerializeField]
    private GameObject m_coinParent;

    private int m_point;
    [SerializeField]
    private TextMeshProUGUI m_pointText;

    [Header("pauseButtons")]
    [SerializeField]
    private GameObject m_pauseButton;
    [SerializeField]
    private GameObject m_ContuwButton;
    [SerializeField]
    private GameObject m_creditsButton;
    [SerializeField]
    private GameObject m_quitButton;



    public static InterFaceManger m_Instance;
    void Awake()
    {
        if(m_Instance == null)
        {
            m_Instance = this;
            m_chosenEmotionPostion = new List<int>();
            m_ChosenEmotion = new List<GameObject>();
            m_chosenEmotionNumber = new List<int>();
        }
        else
        {
            Destroy(gameObject);
        }
    }

    private void Start()
    {
        m_whatEmonion = WhatEmonion.Angry;
        m_winObject.SetActive(false);
        m_losingObject.SetActive(false);
        m_pointText.SetText(m_point.ToString());
    }

    private int Emotions(int _doNot)
    {
        // wil choiose a random emtion that is not the corect 1 
        int _randomnumber = Random.Range(0, m_emotionsButtons.Length);

        if(m_chosenEmotionNumber.Contains(_randomnumber))
        {
            return Emotions(_doNot);
        }
        else
        {
            m_chosenEmotionNumber.Add(_randomnumber);
            return _randomnumber;
        }       
    }

    
    public void CorrectButton(WhatEmonion _whatEmontion)
    {   
        // als die goed is laat die een briegf vallen en je krijgt punt er bij if it is the corect buton let a card drop down and give points
        m_ButtonEmonin = _whatEmontion;
        if(m_whatEmonion == m_ButtonEmonin)
        {
            float _random = Random.Range(500, 1500);
            print(_random);
            Vector3 _spawn = new Vector3(_random,700,0);
            print(_spawn);
            Instantiate(m_coin, _spawn, Quaternion.identity, m_coinParent.transform);
            m_point++;           
        }  
        else
        {
             m_point--;        
        }
        m_pointText.SetText(m_point.ToString());
        
        // checks if you alry won so not reset for new round
        if(m_point >= m_winPoints)
        {
            m_winObject.SetActive(true);
            foreach (GameObject _item in m_ChosenEmotion)
            {
                _item.SetActive(false);
            }
            m_pauseButton.SetActive(false);
            m_creditsButton.SetActive(true);
            m_quitButton.SetActive(true);
        }
        else if(m_point <= m_losingPoints)
        {
            m_losingObject.SetActive(true);
            foreach (GameObject _item in m_ChosenEmotion)
            {
                _item.SetActive(false);
            }
            m_pauseButton.SetActive(false);
            m_creditsButton.SetActive(true);
            m_quitButton.SetActive(true);
        }
        else
        {
            print(m_point);
            ReSetLIst();
            Gamemanger.M_Instance.ResetPerson();
        }      
    }

    private Vector3 ButtenPlaceMent()
    {//tels where the buton placement need to be
        int _randomNumber = Random.Range(0, 3);

        if(_randomNumber == 0 && !m_chosenEmotionPostion.Contains(0))
        {
            m_chosenEmotionPostion.Add(0);
            Vector3 _left = new Vector3(675, 70, 0);
            return _left;
        }
        else if(_randomNumber == 1 && !m_chosenEmotionPostion.Contains(1))
        {
            m_chosenEmotionPostion.Add(1);
            Vector3 _middle = new Vector3(925, 70, 0);
            return _middle;
        }
        else if(_randomNumber == 2 && !m_chosenEmotionPostion.Contains(2))
        {
            m_chosenEmotionPostion.Add(2);
            Vector3 _right = new Vector3(1175, 70, 0);
            return _right;
        }
        else
        {
            return ButtenPlaceMent();
        }
    }

    public void SpawnButton()
    {//place the buton and ask where it need to be placed
        m_whatEmonion = Gamemanger.M_Instance.m_WhatEmonion;
        m_ChosenEmotion.Add( Instantiate(m_emotionsButtons[((int)m_whatEmonion)], ButtenPlaceMent(), Quaternion.identity,m_parentButton.transform));
        m_chosenEmotionNumber.Add(((int)m_whatEmonion));
        for (int i = 0; i < 2; i++)
        {
            m_ChosenEmotion.Add(Instantiate(m_emotionsButtons[Emotions(0)], ButtenPlaceMent(), Quaternion.identity,m_parentButton.transform));
        }
    }

    public void ReSetLIst()
    {
        m_chosenEmotionNumber.Clear();
        m_chosenEmotionPostion.Clear();
        foreach (GameObject item in m_ChosenEmotion)
        {
            GameObject.Destroy(item);
        }
        m_ChosenEmotion.Clear();
    }


    public void PauseGame()
    {
        m_pauseButton.SetActive(false);
        foreach (GameObject item in m_ChosenEmotion)
        {
            item.SetActive(false);
        }
        m_ContuwButton.SetActive(true);
        m_creditsButton.SetActive(true);
        m_quitButton.SetActive(true);
    }

    public void Unpause()
    {
        m_pauseButton.SetActive(true);
        foreach (GameObject item in m_ChosenEmotion)
        {
            item.SetActive(true);
        }
        m_ContuwButton.SetActive(false);
        m_creditsButton.SetActive(false);
        m_quitButton.SetActive(false);
    }

    public void Crdits()
    {

    }

    public void Quit()
    {
        Application.Quit();
    }
}
```
