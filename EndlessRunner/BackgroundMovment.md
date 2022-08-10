```css

public class BackGround : MonoBehaviour
{
    [SerializeField]
    private GameObject backgroundfirst;

    public void moeftoo(GameObject moevegameobect) 
    {  // checks what is the most back background object and places it there
        moevegameobect.transform.position = new Vector3(backgroundfirst.transform.position.x + 55, moevegameobect.transform.position.y, moevegameobect.transform.position.z);
        backgroundfirst = moevegameobect;
    }
}
```
