### psandroidfundafrag
####3 Adding the Fragment to an Activity
#####Adding a Fragment to an Activity by XML
create project->empty activity  
create new java class under java
```
public classs KFragment extends Fragment{
//extend android.app.Fragment (not support.v4)

  @Nullable
  @Override
  public View onCreateView(LayoutInflater inflator, ViewGroup container, Bundle savedInstanceState){
    //return super.onCreateView(inflater,container,savedInstanceState);
    View view = inflater.inflate(R.layout.fragment_ke,container,false);
    return view;
  }
}
```
in activity_main.xml, design mode, select custom->KFragment,->click,select @layout/fragment_k

#####3. Getting Familiar with FragmentManager and FragmentTransaction
FragmentManager - Interface to interact with Fragment objects inside the Activity
FragmentTransaction -API for performing a set of Fragment opeaations such as add, remove, replace
#####4. Adding a Fragment to an Activity Programmatically by JAVA
steps:
- create subclass of Fragment
- create a layout for Fragment
- link layout with fragment subclass
- place the fragment inside an activity
  - initialize fragmentManager
  - initilize fragmentTransaction
  - start add/remove/replace operation
  - commit the transaction  

MainActivity.java
```
...
KFragment kf = new KFragment();
FragmentManager fm = getFragmentManager();
FragementTransaction t = manager.beginTransaction();
t.add(R.id.main,kf,"kFragment");  //main = MainActivity's layout id
t.commit();
```
center a fragment
```
<FrameLayout android:id="@+id/container" android:layout_width="wrap_content" android:layout_height="wrap_content" android:layout_centerInParent="true"/>
```
change previous code
```
t.add(R.id.container,kf,"kFragment");  //main = MainActivity's layout id
//add this fragment to preset FrameLayout id. See 5/3 Performing add and remove 02:00
```

####Exploring Fragment Lifecycle
#####1. Exploring Fragment Lifecycle
onAttach->onCreate->onCreateView(create a view of fragment)->onActivityCreated->onStart(fragment is visible)->onResume(running state)  
Use press back button->onPause->onStop(not visible)->onDestroyView(after onCreateView)->onDestroy(release memory)->onDetach(detach from activity)
#####2 Demo
use
```
onAttach(context) //add in API 23
onAttach(Activity) //deprecated in API23
```
#####3. Exploring Fragment Lifecycle in Context of the Activity Lifecycle
Full lifecycle:  
onCreate(Activity)->onAttach(Fragment)->onAttachAttachment(Activity)->onCreate,onCreateView,onActivityCreated(Fragment)->onStart(Activity)->onStart(Fragment)->onResume(Activity)->onResume(Fragment) 
Use press back button->onPause(Fragment)->onPause(Activity)->onStop(Fragment)->onDestroyView,onDestroy,onDetach(Fragment)->onDestroy(Activity)
####5. Playing Around with Fragment Transactions
#####1. Introduction to Various Types of Fragment Transactions
transaction type: add remove replace attach detach show hide  

#####3. Performing Add and Remove Transactions
To avoid repitition, set
```
FragmentManager fm;
@Override
protected void onCreate(Bundle savedInstanceState){
  fm.getFragmentManager();
}
```
remove fragment
```
KFragment k = (KFragment)manager.findFragmentByTag("kFragment");
FragmentTransaction t= manager.beginTransaction();
if(k!=null){
t.remove(k);
t.commit();
}else{
  Toast.makeText(this,"".Toast.LENGTH_SHORT).show();
}
```
#####4. Exploring Replace Transaction
code snippet
```
KFragment k = new KFragment();
FragmentTransaction t= manager.beginTransaction();
t.replace(R.id.container,k,"kFragment");
t.commit();
```
#####5. Playing with Attach and Detach Transactions
#####6. show/hide
```
t.show/hide/attach/detach(k);
t.commit();
```
[reference](http://stackoverflow.com/questions/9156406/whats-the-difference-between-detaching-a-fragment-and-removing-it)
####06. Sending Data to a Fragment from Parent Activity
#####1.Intro
Pass data from activity to fragment
- Bundle
- Fragment  

#####3.Using Bundle to pass data
snippet, MainActivity
```
public void sendData(View view){
  int first = Integer.valueOf(a.getText().toString());
  int second = Integer.valueOf(a.getText().toString());
  Bundle b = new Bundle();
  b.putInt("first",first);
  b.putInt("second",second);
  KFragment k = new KFragment();
  k.setArguments(b);
  FragmentTransaction t= manager.beginTransaction();
  t.add(R.id.container,k,"kFragment");
  t.commit();
}
```
snippet KFragment
```
onCreateView(LayoutInflater ....{
  Bundle b = getArguments();
  int f = b.getInt("first",0); //0=default
  ..
}
```
#####4. Using Fragment Object and Custom Methods to Pass Data
snippet, MainActivity
```
public void sendData(View view){
  int first = Integer.valueOf(a.getText().toString());
  int second = Integer.valueOf(a.getText().toString());

  KFragment k = new KFragment();
  k.setData(first,second);// or pass non-pri type
  //k.setData(new Klass());
  FragmentTransaction t= manager.beginTransaction();
  t.add(R.id.container,k,"kFragment");
  t.commit();
}
```
snippet KFragment
```
public void setData(int a,int b){
  this.a=a;
  this.b=b;
}
```
####7. Understanding Communication Flow from Fragment to Activity
#####4. Sending Data to Activity from Fragment Using Interface
create a interface MyListener.java(with method addTwoNumbers),then, in KFragment.java
```
private void sendData(){
  MyListener m = (MyListener)getActivity();  //hard to memorize
  m.addTwoNUmbers(a,b);
}
```
in MainActivity.java
```
public void addTwoNumbers(int a, int b){
  int res = a+b;
}
```
####8. Understanding Inter-fragment Communication
#####3. Understanding the Data Flow from One Fragment to Another
similiar to pass data from fragment to activity. just do some change.in MainActivity.java
```
public void add(int x,int y){
  fragB.addTwoNumbers(x,y)
}
```
in FragB.java
```
public void addTwoNumbers(int a, int b){
  int res = a+b;
}
```
#####4. Implementing the Inter-fragment Communication in Project
step:
- Create an interface MyListener
- Implement MyListener in Activity
- Pass data from FragmentA to Activity ``` activity.add(x,y)```
- pass data from Activity to FragmentB ```fragB.addTwoNumbers(x,y)```  

####9. Enabling the Back Button with Backstack
#####1. Overview
Default case is:
- Activity responds to back button
- Fragments are not aware of back button  

```
addToBackStack(null)
addToBackStack(String tag)
popBackStack()
popBackStack(String tag,int FLAG)
```
#####2
in MainActivity.java
```
public class MainActivity extends AppCompatActivity implements OnBackStackChangedListener{
  protected void onCreate(Bundle savedInstanceState){
    ...
    manager.addOnBackStackChangedListener(this);
  }
  ...
  public void addFragment(View v){
    KFragment kf = new KFragment();
  FragementTransaction t = manager.beginTransaction();
  t.add(R.id.container,kf,"kFragment");  
  t.addToBackStack("AddA"); //Note the position, just before commit method
  t.commit();
  }
  public void dummyBack(View v){
    manager.popBackStack();
  }
  
  public void pop_AddA_Inclusive(View v){
    manager.popBackStack("AddA",FragmentManager.POP_BACK_STACK_INCLUSIVE);
  }
  
  public void pop_AddB(View v){
    manager.popBackStack("AddA",0);
  }
  
  @Override
  public void onBackStackChanged(){
    int length = manager.getBackStackEntryCount();
    StringBuilder s = new StringBuilder("");
    for(int i=length-1;i>0;i--){
      s.append(manager.getBackStackEntryAt(i).getName());
    }
    Log.i(TAG,s.toString());
  }
}
```
