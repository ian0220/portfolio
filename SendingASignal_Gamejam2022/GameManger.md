
```css
public class Gamemanger : MonoBehaviour
{
    public static Gamemanger M_Instance;
    private int m_points;
    public WhatEmonion m_WhatEmonion;

    [Header("body")]
    [SerializeField]
    private GameObject[] m_bodies;
    private GameObject m_usingBody;
    [SerializeField]
    private Vector3 m_postionBody;
    [SerializeField]
    private GameObject m_bodyParent;

    [Header("clothes")]
    [SerializeField]
    private GameObject[] m_clothes;// 0 tot 7 is angry 8 tot 11 is shy en 12 tot 17 is standing
    private GameObject m_usingClothes;
    [SerializeField]
    private Vector3 m_clothesPostion;
    [SerializeField]
    private GameObject m_clothesParent;
    private int m_randomClothes;

    [Header("face")]
    [SerializeField]
    private GameObject[] m_faces;
    private GameObject m_usingFace;
    [SerializeField]
    private Vector3 m_postionFace;
    [SerializeField]
    private GameObject m_parentFace;

    [Header("hair")]
    [SerializeField]
    private GameObject[] m_hair;
    private GameObject m_usingHair;
    [SerializeField]
    private Vector3 m_postionHair;
    [SerializeField]
    private GameObject m_parentHair;


    private void Awake()
    {
        if(M_Instance == null)
        {
            M_Instance = this;
        }
        else
        {
            Destroy(gameObject);
        }
        
    }

    void Start()
    {
        m_WhatEmonion = WhatEmonion.Angry; 
        ResetPerson(); // person is the character you have to look at
    }

    public void ResetPerson()
    {
        // here I set the enum random voor a emosion that will be genarated
        m_WhatEmonion = (WhatEmonion)Random.Range(0, 6); 
        Destroy(m_usingBody);
        Destroy(m_usingClothes);
        Destroy(m_usingFace);
        Destroy(m_usingHair);

        IsSpawning();      
    }

    private void IsSpawning()
    {
        int _randomBody = Random.Range(0, m_bodies.Length  );
        // spawn the body that you wil see
        m_usingBody = Instantiate(m_bodies[_randomBody], m_postionBody, Quaternion.identity);
        // place it under a parent so it had the right lajer order 
        if(_randomBody == 0)
        // choos what for clohts it has 
        m_usingBody.transform.SetParent(m_bodyParent.transform); 
        {
             m_randomClothes = Random.Range(0, 4);
        }
        else if(_randomBody == 1)
        {
            m_randomClothes = Random.Range(4, 8);
        }
        else if (_randomBody == 2)
        {
            m_randomClothes = Random.Range(8, 10);
        }
        else if (_randomBody == 3)
        {
            m_randomClothes = Random.Range(10, 12);
        }
        else if (_randomBody == 4)
        {
            m_randomClothes = Random.Range(12, 15);
        }
        else
        {
            m_randomClothes = Random.Range(15, 18);
        }

        m_usingClothes = Instantiate(m_clothes[m_randomClothes], m_clothesPostion, Quaternion.identity, m_clothesParent.transform);
        
        // the face wil spawn with de enum(m_WhatEmonion) is
        m_usingFace = Instantiate(m_faces[((int)m_WhatEmonion)], m_postionFace, Quaternion.identity);
        m_usingFace.transform.SetParent(m_parentFace.transform) ;

        int _randomHair = Random.Range(0, m_hair.Length );
        m_usingHair = Instantiate(m_hair[_randomHair], m_postionHair, Quaternion.identity);
        m_usingHair.transform.SetParent(m_parentHair.transform);

        InterFaceManger.m_Instance.SpawnButton();
    }
}
```
