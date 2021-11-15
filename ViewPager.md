# ViewPager

    废话不多说直接上代码
    
    首先是MainActivity：
      
      我们通过LayoutInflater拿到我们的3个view并将其放到viewList里面，然后findViewById拿到viewPager，创建一个myAdapter，
      然后将其设置给我viewPager
      
            public class MainActivity extends AppCompatActivity {

                @Override
                protected void onCreate(Bundle savedInstanceState) {
                    super.onCreate(savedInstanceState);
                    setContentView(R.layout.activity_main);

                    LayoutInflater layoutInflater = getLayoutInflater().from(this);
                    View view1 = layoutInflater.inflate(R.layout.layout, null);
                    View view2 = layoutInflater.inflate(R.layout.layout1, null);
                    View view3 = layoutInflater.inflate(R.layout.layout2, null);


                    List<View> viewList = new ArrayList<>();
                    viewList.add(view1);
                    viewList.add(view2);
                    viewList.add(view3);

                    ViewPager viewPager = findViewById(R.id.vp);
                    MyAdapter myAdapter = new MyAdapter(viewList);
                    viewPager.setAdapter(myAdapter);
                }
            }

      MyAdapter的实现：
      
        MyAdapter继承PagerAdapter，通过构造函数拿到我们的view数据
         
              
             public class MyAdapter extends PagerAdapter {

                private List<View> mListView;

                public MyAdapter(List<View> mListView) {  //构造函数
                    this.mListView = mListView;
                }

                /*
                * 1.将给定位置的view添加到ViewGroup中，创建并显示出来
                * 2.返回一个代表新增页面的object（key），通常都是直接返回view本身就可以了，当然你也可以自定义
                * 自己的key，但是key和每个View要一一对应的关系
                * */
                @NonNull
                @Override
                public Object instantiateItem(@NonNull ViewGroup container, int position) {
                    container.addView(mListView.get(position), 0);
                    return mListView.get(position);
                }

                /*
                * 获取ViewPager中有多少个view
                * */
                @Override
                public int getCount() {
                    return mListView.size();
                }

                /*
                * 判断iinstantiateItem函数返回的key与一个页面视图是否是代表的同一个视图
                * （即他俩是否是相对应的，对应的表示同一个view），通常我们直接return view == onject
                * */
                @Override
                public boolean isViewFromObject(@NonNull View view, @NonNull Object object) {
                    return view == object;
                }

                /*
                 * 移除一个给定位置的页面。适配器有责任从容器中删除这个视图。这是为了确保在
                 * finishUpdate（ViewGroup）返回时视图能够被移除。
                 * 而另外2个方式则是涉及到一个key的东东
                 * */
                @Override
                public void destroyItem(@NonNull ViewGroup container, int position, @NonNull Object object) {
                   container.removeView(mListView.get(position ));
                }
            }






