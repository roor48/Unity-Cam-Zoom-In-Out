# Unity-Cam-Zoom-In-Out

using UnityEngine;

public class CameraLogic : MonoBehaviour
{
    [SerializeField] private Transform camObj;
    private float xRotate, yRotate, xRotateMove, yRotateMove;
    [SerializeField] private float rotSpeed;
    [SerializeField] private float zoomSpeed;

    private void Update()
    {
        CamRot();
        ZoomCam();
    }

    private void CamRot()
    {
        if (!Input.GetMouseButton(1))
            return;
        xRotateMove = -Input.GetAxis("Mouse Y") * Time.deltaTime * rotSpeed;
        yRotateMove = Input.GetAxis("Mouse X") * Time.deltaTime * rotSpeed;

        yRotate = transform.eulerAngles.y + yRotateMove;
        //xRotate = transform.eulerAngles.x + xRotateMove;
        xRotate += xRotateMove;

        xRotate = Mathf.Clamp(xRotate, -90, 90); // 위, 아래 고정

        camObj.eulerAngles = new Vector3(xRotate, yRotate, 0);
    }

    private void ZoomCam()
    {
        float wheelInput = Input.GetAxis("Mouse ScrollWheel");
        if (wheelInput == 0)
            return;

        float distance = Vector3.Distance(transform.position, camObj.position);

        switch (distance)
        {
            case > 2 when wheelInput > 0: // Wheel Up
                Debug.Log("Wheel Up");
                transform.position += zoomSpeed * transform.forward;
                break;
            
            case < 15 when wheelInput < 0: // Wheel Down
                Debug.Log("Wheel Down");
                transform.position += -zoomSpeed * transform.forward;
                break;
        }
    }
}
