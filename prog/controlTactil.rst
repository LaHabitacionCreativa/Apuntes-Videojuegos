==========================
Control táctil
==========================


Swipe
====================

.. code-block:: c#

    using UnityEngine;
    using System.Collections;

    public class Swipe : MonoBehaviour {


        private Vector2 dir = Vector2.zero;
        private Vector2 touchOrigin = -Vector2.one;


        // Use this for initialization
        void Start () {
        
        }
        
        // Update is called once per frame
        void Update () {

            // Control por teclado si ejecutamos en editor, escritorio o web
            
            #if UNITY_EDITOR || UNITY_STANDALONE || UNITY_WEBPLAYER
            
            float x = (int) (Input.GetAxisRaw ("Horizontal"));
            float y = (int) (Input.GetAxisRaw ("Vertical"));
            //Si nos movemos en horizontal marcamos la vertical a 0
            if(x != 0) y = 0;
            
            if (x > 0) dir = Vector2.right;
            else if (x < 0) dir = -Vector2.right;
            else if (y > 0) dir = Vector2.up;
            else if (y < 0) dir = -Vector2.up; 
            
            // Control con gestos si ejecutamos en dispositivos moviles
            
            #elif UNITY_IOS || UNITY_ANDROID || UNITY_WP8 || UNITY_IPHONE
            
            if (Input.touchCount > 0)
            {
                //Guardamos el primer toque
                Touch myTouch = Input.touches[0];
                
                //Si comienza un arrastre cogemos la posicion de comienzo
                if (myTouch.phase == TouchPhase.Began)
                {
                    touchOrigin = myTouch.position;
                }
                
                //Si finaliza el arrastre
                else if (myTouch.phase == TouchPhase.Ended && touchOrigin.x >= 0)
                {
                    //Posicion de final del toque
                    Vector2 touchEnd = myTouch.position;
                    
                    //Diferencia entre el principio y final del arrastre
                    float x = touchEnd.x - touchOrigin.x;
                    float y = touchEnd.y - touchOrigin.y;
                    
                    //No repetimos hasta que se origine un nuevo arrastre
                    touchOrigin = -Vector2.one;
                    
                    //Si el arrastre ha sido muy corto (menos de 8 px)
                    //ha sido una pulsacion que descartamos
                    if(Mathf.Abs(x) > 8 || Mathf.Abs(y) > 8){ 
                        //Obtenemos la direccion del arrastre
                        if (Mathf.Abs(x) > Mathf.Abs(y)){
                            if(x>0) dir = Vector2.right;
                            else dir = -Vector2.right;
                        }
                        else{
                            if(y>0) dir = Vector2.up;
                            else dir = -Vector2.up;
                        }
                    }                  
                }
            }		
            #endif

            // Movemos el objeto
            transform.Translate (dir.x * Time.deltaTime, 
                                0, 
                                dir.y * Time.deltaTime);
        }
    }



​Click
========

.. code-block:: c#

    using UnityEngine;
    using System.Collections;

    public class Click : MonoBehaviour {


        private Vector3 a, b;
        public float velocidad = 10f;

        void Start () {
        }
        
        void Update () {
            //moverDirecto();
            //moverGradual();
            seguirRaton();
        }

        void moverDirecto(){
            a = transform.position;
            if (Input.GetMouseButtonDown(0))
            {
                RaycastHit hit;
                Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
                if (Physics.Raycast(ray, out hit))
                {
                    b.x = hit.point.x;
                    b.z = hit.point.z;
                    //dejo la misma y ya que no quiero que mueva en altura
                    b.y = transform.position.y;

                    transform.position = b;
                }
            }
        }

        void moverGradual(){
            a = transform.position;

            if (Input.GetMouseButtonDown(0))
            {
                RaycastHit hit;
                Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
                if (Physics.Raycast(ray, out hit))
                {
                    b.x = hit.point.x;
                    b.z = hit.point.z;
                    //dejo la misma y ya que no quiero que mueva en altura
                    b.y = transform.position.y;

                    b = b - a;
                    b.Normalize();
                    b = b * velocidad + a;
                    transform.position = Vector3.Lerp( a, b, Time.deltaTime);
                }
            }
        }
        
        void seguirRaton(){
            a = transform.position;
            RaycastHit hit;
            Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
            if (Physics.Raycast(ray, out hit))
            {
                b.x = hit.point.x;
                b.z = hit.point.z;
                //dejo la misma y ya que no quiero que mueva en altura
                b.y = transform.position.y;

                b = b - a;
                b.Normalize();
                b = b * velocidad + a;
                transform.position = Vector3.Lerp( a, b, Time.deltaTime);
            }
        }
    }



Joystick
================

Joystick.cs
--------------

.. code-block:: c#

    using UnityEngine;
    using UnityEngine.UI;
    using UnityEngine.Events;
    using UnityEngine.EventSystems;
    using System.Collections;

    #if UNITY_EDITOR
    using UnityEditor;
    #endif

    namespace UnityEngine.UI {
        [AddComponentMenu("UI/Joystick", 36), RequireComponent(typeof(RectTransform))]
        public class Joystick : UIBehaviour, IBeginDragHandler, IEndDragHandler, IDragHandler {

            [SerializeField, Tooltip("The child graphic that will be moved around")]
            RectTransform _joystickGraphic;
            public RectTransform JoystickGraphic {
                get { return _joystickGraphic; }
                set {
                    _joystickGraphic = value;
                    UpdateJoystickGraphic();
                }
            }

            [SerializeField]
            Vector2 _axis;

            [SerializeField, Tooltip("How fast the joystick will go back to the center")]
            float _spring = 25;
            public float Spring {
                get { return _spring; }
                set { _spring = value; }
            }

            [SerializeField,  Tooltip("How close to the center that the axis will be output as 0")]
            float _deadZone = .1f;
            public float DeadZone {
                get { return _deadZone; }
                set { _deadZone = value; }
            }

            [Tooltip("Customize the output that is sent in OnValueChange")]
            public AnimationCurve outputCurve = new AnimationCurve(new Keyframe(0, 0, 1, 1), new Keyframe(1, 1, 1, 1));

            public JoystickMoveEvent OnValueChange;

            public Vector2 JoystickAxis {
                get {
                    Vector2 outputPoint = _axis.magnitude > _deadZone ? _axis : Vector2.zero;
                    float magnitude = outputPoint.magnitude;

                    outputPoint *= outputCurve.Evaluate(magnitude);

                    return outputPoint;
                }
                set { SetAxis(value); }
            }

            RectTransform _rectTransform;
            public RectTransform rectTransform {
                get {
                    if(!_rectTransform) _rectTransform = transform as RectTransform;

                    return _rectTransform;
                }
            }

            bool _isDragging;

            [HideInInspector]
            bool dontCallEvent;

            public void OnBeginDrag(PointerEventData eventData) {
                if(!IsActive())
                    return;

                EventSystem.current.SetSelectedGameObject(gameObject, eventData);

                Vector2 newAxis = transform.InverseTransformPoint(eventData.position);

                newAxis.x /= rectTransform.sizeDelta.x * .5f;
                newAxis.y /= rectTransform.sizeDelta.y * .5f;

                SetAxis(newAxis);

                _isDragging = true;
                dontCallEvent = true;
            }

            public void OnEndDrag(PointerEventData eventData) {
                _isDragging = false;
            }

            public void OnDrag(PointerEventData eventData) {
                RectTransformUtility.ScreenPointToLocalPointInRectangle(rectTransform, eventData.position, eventData.pressEventCamera, out _axis);

                _axis.x /= rectTransform.sizeDelta.x * .5f;
                _axis.y /= rectTransform.sizeDelta.y * .5f;

                SetAxis(_axis);

                dontCallEvent = true;
            }

            void OnDeselect() {
                _isDragging = false;
            }


            void Update() {
                if(_isDragging)
                    if(!dontCallEvent)
                        if(OnValueChange != null) OnValueChange.Invoke(JoystickAxis);
            }

            void LateUpdate() {
                if(!_isDragging)
                    if(_axis != Vector2.zero) {
                        Vector2 newAxis = _axis - (_axis * Time.unscaledDeltaTime * _spring);

                        if(newAxis.sqrMagnitude <= .0001f)
                            newAxis = Vector2.zero;

                        SetAxis(newAxis);
                    }

                dontCallEvent = false;
            }
            protected override void OnValidate() {
                base.OnValidate();
                UpdateJoystickGraphic();
            }


            public void SetAxis(Vector2 axis) {
                _axis = Vector2.ClampMagnitude(axis, 1);

                Vector2 outputPoint = _axis.magnitude > _deadZone ? _axis : Vector2.zero;
                float magnitude = outputPoint.magnitude;

                outputPoint *= outputCurve.Evaluate(magnitude);

                if(!dontCallEvent)
                    if(OnValueChange != null)
                        OnValueChange.Invoke(outputPoint);

                UpdateJoystickGraphic();
            }

            void UpdateJoystickGraphic() {
                if(_joystickGraphic)
                    _joystickGraphic.localPosition = _axis * Mathf.Max(rectTransform.sizeDelta.x, rectTransform.sizeDelta.y) * .5f;
            }

            [System.Serializable]
            public class JoystickMoveEvent : UnityEvent<Vector2> { }
        }
    }

    #if UNITY_EDITOR
    static class JoystickGameObjectCreator {
        [MenuItem("GameObject/UI/Joystick", false, 2000)]
        static void Create() {
            GameObject go = new GameObject("Joystick", typeof(Joystick));

            Canvas canvas = Selection.activeGameObject ? Selection.activeGameObject.GetComponent<Canvas>() : null;

            Selection.activeGameObject = go;

            if(!canvas)
                canvas = Object.FindObjectOfType<Canvas>();

            if(!canvas) {
                canvas = new GameObject("Canvas", typeof(Canvas), typeof(RectTransform), typeof(GraphicRaycaster)).GetComponent<Canvas>();
                canvas.renderMode = RenderMode.ScreenSpaceOverlay;
            }

            if(canvas)
                go.transform.SetParent(canvas.transform, false);

            GameObject background = new GameObject("Background", typeof(Image));
            GameObject graphic = new GameObject("Graphic", typeof(Image));

            background.transform.SetParent(go.transform, false);
            graphic.transform.SetParent(go.transform, false);

            background.GetComponent<Image>().color = new Color(1, 1, 1, .86f);

            RectTransform backgroundTransform = graphic.transform as RectTransform;
            RectTransform graphicTransform = graphic.transform as RectTransform;

            graphicTransform.sizeDelta = backgroundTransform.sizeDelta * .5f;

            Joystick joystick = go.GetComponent<Joystick>();
            joystick.JoystickGraphic = graphicTransform;
        }
    }
    #endif


MoveObject.cs
------------------

.. code-block:: c#

    using UnityEngine;
    using System.Collections;

    [RequireComponent(typeof(Rigidbody))]
    public class MoveObject : MonoBehaviour {

        private Rigidbody compo;

        void Start() {
            compo=GetComponent<Rigidbody>();
        }

        public void Move(Vector2 axis) {
            compo.AddForce(new Vector3(axis.x, 0, axis.y) * Time.deltaTime * 1000, ForceMode.Force);
        }
    }


Perseguir
===============

.. code-block:: c#

    using UnityEngine;
    using System.Collections;

    public class Perseguir : MonoBehaviour {
        private Vector3 a, b;
        public float velocidad = 10f;
        public GameObject objetivo;

        void Start () {
        }
        
        void Update () {
            transform.position = Vector3.MoveTowards(transform.position, 
                                                    objetivo.transform.position, 
                                                    velocidad * Time.deltaTime);
            transform.LookAt (objetivo.transform);
        }	
    }


Seguir ratón
===================

.. code-block:: c#

    using UnityEngine;
    using System.Collections;

    public class SeguirRaton : MonoBehaviour {

        private Vector3 a, b;
        public float velocidad = 10f;

        void Start () {
        }
        
        void Update () {
            //moverDirecto();
            //moverGradual();
            seguirRaton();
        }

        void moverDirecto(){
            a = transform.position;
            if (Input.GetMouseButtonDown(0))
            {
                RaycastHit hit;
                Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
                if (Physics.Raycast(ray, out hit))
                {
                    b.x = hit.point.x;
                    b.z = hit.point.z;
                    //dejo la misma y ya que no quiero que mueva en altura
                    b.y = transform.position.y;

                    transform.position = b;
                }
            }
        }

        void moverGradual(){
            a = transform.position;

            if (Input.GetMouseButtonDown(0))
            {
                RaycastHit hit;
                Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
                if (Physics.Raycast(ray, out hit))
                {
                    b.x = hit.point.x;
                    b.z = hit.point.z;
                    //dejo la misma y ya que no quiero que mueva en altura
                    b.y = transform.position.y;

                    b = b - a;
                    b.Normalize();
                    b = b * velocidad + a;
                    transform.position = Vector3.Lerp( a, b, Time.deltaTime);
                }
            }
        }
        
        void seguirRaton(){
            a = transform.position;
            RaycastHit hit;
            Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
            if (Physics.Raycast(ray, out hit))
            {
                b.x = hit.point.x;
                b.z = hit.point.z;
                //dejo la misma y ya que no quiero que mueva en altura
                b.y = transform.position.y;

                b = b - a;
                b.Normalize();
                b = b * velocidad + a;
                transform.position = Vector3.Lerp( a, b, Time.deltaTime);
            }
        }
    }
