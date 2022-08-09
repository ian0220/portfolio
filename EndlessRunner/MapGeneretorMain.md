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
    // Start is called before the first frame update
    void Start()
    {
        m_enemyPostionchoose = 0;
        //Instantiate(m_floor, new Vector3(m_floor.transform.position.x + m_floor.GetComponent<MeshRenderer>().bounds.size.x, m_floor.transform.position.y, m_floor.transform.position.z), Quaternion.identity);
        for (int i = 0; i < m_aantal; i++)
        {
            m_Lastspawned = Instantiate(Floor, new Vector3(Floor.transform.position.x + Floor.GetComponent<MeshRenderer>().bounds.size.x * i, Floor.transform.position.y, Floor.transform.position.z), Quaternion.identity);
        }
    }

    public void nextspawn()
    {   // spaned achter de achterste
        m_Lastspawned = Instantiate(Floor, new Vector3(m_Lastspawned.transform.position.x + m_Lastspawned.GetComponent<MeshRenderer>().bounds.size.x - 2, m_Lastspawned.transform.position.y, m_Lastspawned.transform.position.z), Quaternion.identity);

    }

    private void Block()
    {
       
        m_enemyPostionchoose = Random.Range(0, 3);  // random gemaakt om postie te bekijken
        if(m_enemyPostionchoose == 2) // controleerd welke postion het is
        {
            Vector3 _bovenPostion = new Vector3(40, 12, 0);  // zet hem op de postion waar die spanwend
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
       
        m_currenyPostionchoose = Random.Range(0, 3);  // random gemaakt om postie te bekijken
        if(m_currenyPostionchoose == m_enemyPostionchoose)
        {
            return;
        }
        if(m_currenyPostionchoose == 2) // controleerd welke postion het is
        {
            Vector3 _bovenPostion = new Vector3(40, 12, 0);  // zet hem op de postion waar die spanwend
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
        if(m_timerenemyspawnen <= 0) // hier kijkt die of de secondes voor bij zijn en dat die dan 1 mag gaan spawnen
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
    {  // dit zet de objecten terug naar de object pool
        PoolItem _poolenemy = m_enemypool.GetComponent<PoolItem>();
        _poolenemy.ReturnToPool();
    }
}
```
