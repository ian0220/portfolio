```css

public class MapGeneretorMain : MonoBehaviour
{
    [SerializeField]
    public GameObject Floor;
    private int m_aantal = 3;
    private GameObject m_Lastspawned;
    private int m_enemyPostionchoose;
    [SerializeField]
    private GameObject m_vuurBal;
    private float m_timerenemyspawnen = 3;

    [SerializeField]
    private GameObject m_currency;
    private float m_timerCurrenySpawn;
    private int m_currenyPostionchoose;

    [SerializeField]
    private ObjectPool m_enemypool;
    [SerializeField]
    private ObjectPool m_currencyPool;

    private Position m_enumpostion;
    
    void Start()
    {
        m_enemyPostionchoose = 0;
        // spawn new floor
        for (int i = 0; i < m_aantal; i++)
        {
            m_Lastspawned = Instantiate(Floor, new Vector3(Floor.transform.position.x + Floor.GetComponent<MeshRenderer>().bounds.size.x *i, Floor.transform.position.y, Floor.transform.position.z), Quaternion.identity);
        }
    }

    public void nextspawn()
    {   // spawned floor to the back floor
        m_Lastspawned = Instantiate(Floor, new Vector3(m_Lastspawned.transform.position.x + m_Lastspawned.GetComponent<MeshRenderer>().bounds.size.x - 2, m_Lastspawned.transform.position.y, m_Lastspawned.transform.position.z), Quaternion.identity);

    }

    private void Block()
    {
       // lookat for out of 3 postion and let a enemy spawn on 1 of the postions
        m_enemyPostionchoose = Random.Range(0, 3);  
        if(m_enemyPostionchoose == 2) 
        {
            Vector3 _bovenPostion = new Vector3(40, 12, 0);  
            m_enemypool.GetPooledObject(transform.position = _bovenPostion, transform.rotation, null);
        }
        else if(m_enemyPostionchoose == 1)
        {
            Vector3 _middenpostion = new Vector3(40, 7, 0);
            m_enemypool.GetPooledObject(transform.position = _middenpostion, transform.rotation, null);
        }
        else if(m_enemyPostionchoose == 0)
        {
            Vector3 _onderpostion = new Vector3(40, 3, 0);
            m_enemypool.GetPooledObject(transform.position = _onderpostion, transform.rotation, null);
        }
    }

     private void CurrencySpawner()
     {
        // lookat for out of 3 postion and let a currency spawn on 1 of the postions
        m_currenyPostionchoose = Random.Range(0, 3); 
        if(m_currenyPostionchoose == m_enemyPostionchoose)
        {
            return;
        }
        if(m_currenyPostionchoose == 2) 
        {
            Vector3 _bovenPostion = new Vector3(40, 12, 0);  
            m_currencyPool.GetPooledObject(transform.position = _bovenPostion, transform.rotation, null);
        }
        else if(m_currenyPostionchoose == 1)
        {
            Vector3 _middenpostion = new Vector3(40, 7, 0);
            m_currencyPool.GetPooledObject(transform.position = _middenpostion, transform.rotation, null);
        }
        else if(m_currenyPostionchoose == 0)
        {
            Vector3 _onderpostion = new Vector3(40, 3, 0);
            m_currencyPool.GetPooledObject(transform.position = _onderpostion, transform.rotation, null);
        }
     }

    private void Update()
    {
        m_timerenemyspawnen -= Time.deltaTime;
        m_timerCurrenySpawn -= Time.deltaTime;
        if(m_timerenemyspawnen <= 0) 
        {
            m_timerenemyspawnen = 1;
            Block();
        }

        if(m_timerCurrenySpawn <= 0)
        {
            m_timerCurrenySpawn = 10;
            CurrencySpawner();
        }
    }

    public void BackToObjectPoolEnemy()
    {  // set the object of a objectpool back to the objectpool
        PoolItem _poolenemy = m_enemypool.GetComponent<PoolItem>();
        _poolenemy.ReturnToPool();
    }
}
```
