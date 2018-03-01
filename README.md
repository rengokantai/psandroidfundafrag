# psandroidfundafrag
## 2. Introduction
### 2 Why Fragment
Reuse the same Fragment across several Activities
## 3. Adding the Fragment to an Activity
### 2 Adding a Fragment to an Activity by XML
steps
- Create a subclass of Fragment  

create project->empty activity  
create new java class under java
```
public classs KFragment extends Fragment{
//extend android.app.Fragment (not support.v4)

  @Nullable
  @Override
  public View onCreateView(LayoutInflater inflator, ViewGroup container, Bundle savedInstanceState){
    //return super.onCreateView(inflater,container,savedInstanceState);
    View view = inflater.inflate(R.layout.fragment_ke,container,false); //attach to root?
    return view;
  }
}
```
in activity_main.xml, design mode, select custom->KFragment,->click,select @layout/fragment_k
```
tools:layout="@layout/fragment_hello"/>
```
### 3 Getting Familiar with FragmentManager and FragmentTransaction
FragmentManager - Interface to interact with Fragment objects inside the Activity
FragmentTransaction -API for performing a set of Fragment opeaations such as add, remove, replace
### 4 Adding a Fragment to an Activity Programmatically by JAVA
steps:
- Create subclass of Fragment
- Create a layout for Fragment
- Link layout with fragment subclass
  - Override onCreateView()
- place the fragment inside an activity
  - Initialize FragmentManager
  - Initilize FragmentTransaction
  - Start add/remove/replace operation
  - Commit the transaction  

MainActivity.java
```

KFragment kf = new KFragment();
FragmentManager fm = getFragmentManager();
FragementTransaction transaction = manager.beginTransaction();
transaction.add(R.id.main,kf,"kFragment");  //main = MainActivity's layout id
transaction.commit();
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

## 4. Exploring Fragment Lifecycle
### 1 Exploring Fragment Lifecycle
onAttach->onCreate->onCreateView(create a view of fragment)->onActivityCreated->onStart(fragment is visible)->onResume(running state)  
Use press back button->onPause->onStop(not visible)->onDestroyView(after onCreateView)->onDestroy(release memory)->onDetach(detach from activity)
### 2 Demo: Fragment Lifecycle
use
```
onAttach(context) //add in API 23
onAttach(Activity) //deprecated in API23
```
### 3 Exploring Fragment Lifecycle in Context of the Activity Lifecycle
Full lifecycle:  
onCreate(Activity)->onAttach(Fragment)->onAttachAttachment(Activity)->onCreate,onCreateView,onActivityCreated(Fragment)->onStart(Activity)->onStart(Fragment)->onResume(Activity)->onResume(Fragment) 
Use press back button->onPause(Fragment)->onPause(Activity)->onStop(Fragment)->onDestroyView,onDestroy,onDetach(Fragment)->onDestroy(Activity)
### 4 Demo

### 5 Summary
onAttach
- The fragment has been attached to the Activity
- Provides the reference to the context of Activity
onCreate
- Initialize essential components
onCreateView
- Fragment draws its User Interface
- Return
  - View: Fragments layout root view
  - Null: If Fragment has no UI

onPause
- Indication of User leaving the Fragment
- Fragment is still visible
- Save the state of Fragment

onStop
- Fragment is not visible
- Either host Activity has been stopped or the fragment has been removed and added to back stack

onDestroyView
- Opposite to onCreateView
- The View hierarchy of the fragment is removed

onDestroy
- Final cleanup of Fragment's state
onDetach
- Fragment has been disassocited from its hosting Activity


## 5. Playing Around with Fragment Transactions
### 1 Introduction to Various Types of Fragment Transactions
transaction type: add remove replace attach detach show hide  

### 3 Performing Add and Remove Transactions
create a frameLayout in MainActivity,set id to container,then
```
FragmentA fragmentA = new FragmentA();
transaction.add(R.id.container,fragmentA,"fragA")
```
###### 03:08
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
KFragment k = (KFragment)manager.findFragmentByTag("fragA");
FragmentTransaction t= manager.beginTransaction();
if(k!=null){
t.remove(k);
t.commit();
}else{
  Toast.makeText(this,"Error",Toast.LENGTH_SHORT).show();//If called in fragment class, this->getActivity()
}
```
### 4 Exploring Replace Transaction
code snippet
```
KFragment k = new KFragment();
FragmentTransaction t= manager.beginTransaction();
t.replace(R.id.container,k,"kFragment");
t.commit();
```
### 5 Playing with Attach and Detach Transactions
does not perform onDestroy and onDetach method.  
The fragment is not destroyed, but just not visible.
### 6 Hiding and Showing a Fragment
no state change
```
t.show/hide/attach/detach(k);
t.commit();
```
[reference](http://stackoverflow.com/questions/9156406/whats-the-difference-between-detaching-a-fragment-and-removing-it)
## 6. Sending Data to a Fragment from Parent Activity
### 1 Introduction
Pass data from activity to fragment
- Bundle
- Fragment  

### 3 Using Bundle to Pass Data from Parent Activity to Fragment
snippet, MainActivity
```
public void sendDataToFragment(View view){
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
### 4 Using Fragment Object and Custom Methods to Pass Data
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
### 4 Using Fragment Object and Custom Methods to Pass Data
snippet KFragment
```
public void setData(int a,int b){
  this.a=a;
  this.b=b;
}
```
###7. Understanding Communication Flow from Fragment to Activity
####4. Sending Data to Activity from Fragment Using Interface
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
###8. Understanding Inter-fragment Communication
####3. Understanding the Data Flow from One Fragment to Another
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
####4. Implementing the Inter-fragment Communication in Project
step:
- Create an interface MyListener
- Implement MyListener in Activity
- Pass data from FragmentA to Activity ``` activity.add(x,y)```
- pass data from Activity to FragmentB ```fragB.addTwoNumbers(x,y)```  

###9. Enabling the Back Button with Backstack
####1. Overview
Default case is:
- Activity responds to back button
- Fragments are not aware of back button  

```
addToBackStack(null)
addToBackStack(String tag)
popBackStack()
popBackStack(String tag,int FLAG)
```
####2
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
####3 
add(even the transaction is "hide", it still count as a add to stack.
```
addToBackStack("addA");
addToBackStack("hideA");
```
new method in MainActivity.java
```
@Override
public void onBackPressed(){
  if(manager.getBackStackEntryCount()>0){
    manager.popBackStack();
  }else{
    super.onBackPressed();
  }
}
```
pop
```
manager.popBackStack("name",0);  //pop all item until this item(not inclusive)
manager.popBackStack("name",POP_BACK_STACK_INCLUSIVE);  //pop all item until this item(inclusive)
manager.popBackStack(); //only last one
```
####5. Making Multiple Changes in a Single FragmentTransaction

```
t.add(R.id.a,...
t.add(R.id.b,...
t.addToBackStack("Add two");
t.commit();
```
###10. Providing Stability to Fragment on Screen Rotation
####2. Fragment Lifecycle Behavior on Screen Rotation
####4. Impact of Screen Rotation on Views and Variables
Views do not retain its state on screen rotation:
- TextView
- Button  

####5. How to Restore Back the Fragment's State
onPause->onSaveInstanceState->onStop

####6. Providing Stability to Fragment on Screen Rotation
Fragment.java
```
@Override
public void onSaveInstanceState(Bundle b){
  super.onSaveInstanceState(b);
  b.putInt("var",var);
}
```
for retrive, you can put in
```
onCreate
onCreateView
onActivityCreated
```
