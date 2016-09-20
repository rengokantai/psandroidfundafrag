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
