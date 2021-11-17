# Fragment

###  1.具备生命周期，子Activity
  
  生命周期：
  
    onAttach()  在fragment和Activity建立关联是调用（Activity传递到此方法内）即绑定
        
    onCreate()  Fragment创建，可以解析Activity传过来的Bundle

    onCreateView() Fragment创建视图时调用

    onActivityCreated()  意味着Activity创建了，在相关联的Activity的onCreate()方法已返回时调用

    onStart() 

    onResume()  此时Fragment is active

    onPause()

    onStop()

    onDestroyView()  当Fragment的视图被移除时调用

    onDestory() 

    onDetach()   当Fragment和Activity取消关联时调用 即解绑
 
###  2.必须委托在Activity才能运行

    当Activity加入Fragment,Activity和Fragment生命周期执行过程

    1）没有加入栈

        (1).打开界面：F onAttach() -> F onCreate() -> F onCreateView() -> A onCreate() -> F onActivityCreated() -> A onStart()
        -> F onStart() -> A onResume() -> F onResume()

        (2).按下Home键： F onPause() -> A onPause() -> F onStop() -> A onStop()

        (3).重新打开页面： A on Restart() -> A onStart() -> F onStart() -> A onResume() -> F onResume()

        (4).后退键： F onPause() -> A onPause() -> F onStop() -> A onStop() -> F onDestoryView() -> F onDestory()
        -> F onDetach() -> A onDestory()

    2）加入栈 
        (1).切换Fragment： F onPause() -> A onPause() -> F onStop() -> A onStop() —> F onDestoryView()

        (2).切换Fragment按后退键： F onCreateView() -> A onCreate() -> F onActivityCreated() -> A onStart()
        -> F onStart() -> A onResume() -> F onResume()

        (3).杀掉进程： F onDestory() -> F onDetach() -> A onDestory()

        (4).切换Fragment后再切换到之前的Fragment（不是返回操作）: 和打开界面一样

###  3.动态添加Fragment

    1）创建一个待处理的fragment

    2）获取FragmentManager，一般都是通过getSupportFragmentManager()

    3）开启一个事物transaction，一般调用fragmentManager的benginTransaction()

    4）使用transaction进行fragment的替换

    5）提交事物

        private void replaceFragment(Fragment fragment) {
            FragmentManager fragmentManager = getSupportFragmentManager();
            FragmentTransaction transaction = fragmentManager.beginTransaction();
            transaction.replace(R.id.frameLayout, fragment);
            transaction.addToBackStack(null);  //fragment加入栈
            transaction.commit();
        }

### 4.Activity发送消息给Fragment
    
    原生方案: Bundle

      1) Activiy发送消息

      Bundle bundle = new Bundle();
      bundle.putString("message", "android");
      BlankFragment bf = new BlankFragment();
      bf.setArguments(bundle);
      replaceFragment(bf);

      2）Fragment里获取

        Bundle bundle = getArgument();
        String string = bundle.getString();

### 5.Fragment和Activity通信的接口方案

    思路： 在Activity创建一个接口对象，将这个对象赋值给Fragment，这时候两者之间就形成一个弱关联。

    首先在Activity里面new一个接口对象IFragmentCallBack，但是IFragmentCallBack是一个抽象类无法被初始化。
    这时候要建立两者的通信，需要去Fragment里面定义一个callBack对象，这个对象由Activity赋值，需要定义一个public函数去承接接口对象，
    然后在A里通过fragment对象去调用public函数，赋的是一个接口类型对象，这时候可以new一个接口类型对象。


      1)定义一个接口IFragmentCallBack,然后定义2个函数去setMessage和getMessage

          public interface IFragmentCallBack {
              void setMessageToActivity(String string);
              String getMessageFromActivity(String msg);
          }

      2）Fragmnet里面

            private IFragmentCallBack iFragmentCallBack;

            public void setiFragmentCallBack(IFragmentCallBack callBack) {  //承接接口类型对象
                iFragmentCallBack = callBack;
            }

            @Override
            public View onCreateView(LayoutInflater inflater, ViewGroup container,
                                    Bundle savedInstanceState) {
                if (root == null) {
                    root =  inflater.inflate(R.layout.fragment_blank, container, false);
                }
                Button btn = root.findViewById(R.id.btn3);
                btn.setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View v) {
                      // iFragmentCallBack.setMessageToActivity("hello, I am Fragment");
                      String string = iFragmentCallBack.getMessageFromActivity("null");
                      Toast.makeText(BlankFragment.this.getContext(), string, Toast.LENGTH_LONG).show();
                    }
                });
                return root;
            }


      3）Activity里面
          BlankFragment bf = new BlankFragment();
          bf.setiFragmentCallBack(new IFragmentCallBack() {
              @Override
              public void setMessageToActivity(String string) {
                  Toast.makeText(MainActivity.this, string, Toast.LENGTH_SHORT).show();
              }

              @Override
              public String getMessageFromActivity(String msg) {
                  return " hello， I am Activity";
              }
          });



### 其他方案（高阶必备）

    需求： Activity更新一个消息，Fragment感兴趣才收到，不感兴趣收不到
    
    eventBus,LiveData..., 这些方案都包含了一个设计模式：观察者模式（发布订阅模式）




