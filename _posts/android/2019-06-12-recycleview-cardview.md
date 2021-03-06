---
layout: post
title: 안드로이드 RecycleView로 CardView 구현하기 (Kotlin)
excerpt: "Android"
tags: [android, programming, thread, handler]
permalink: /android/:year/:month/:day/:title/
category : 안드로이드
---

RecycleView란 제한된 화면에 대용량 데이터를 보여줄 때 사용하고 ListView와 비슷하다.  

CardView를 Recycleview와 함께 사용하는 이유는 일반적으로 리스트 형태로 보여지기 때문에 뷰에 대한 재사용이 필요하기 때문이다.  

또한, LayoutManager의 orientation을 사용하면 child view를 가로나 세로로 배치하여 보여줄 수 있다.  

1. build.gradle에 recyclerview와 cardview library 추가
<pre class="prettyprint">
    implementation 'com.android.support:recyclerview-v7:28.0.0'
    implementation 'com.android.support:cardview-v7:28.0.0'
</pre>

2. fragment 안에 layout으로 들어갈 RecyclerView를 정의 (recycle_fragment_layout.xml)
<pre class="prettyprint">
&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt; &lt;LinearLayout xmlns:android=&quot;http://schemas.android.com/apk/res/android&quot; android:id=&quot;@+id/parent_layout&quot; android:layout_width=&quot;match_parent&quot; android:layout_height=&quot;match_parent&quot; android:orientation=&quot;horizontal&quot;&gt; &lt;android.support.v7.widget.RecyclerView android:id=&quot;@+id/recyclerview&quot; android:layout_width=&quot;match_parent&quot; android:layout_height=&quot;match_parent&quot;/&gt; &lt;/LinearLayout&gt;
</pre>

1. RelativeLayout을 상위 루트로 하는 CardView 레이아웃 파일을 정의한다.  
이때 ReleativeLayout으로 감싸지 않으면 cardView의 magrin이 적용되지 않으니 주의하자.  

<pre class="prettyprint">
&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt; &lt;RelativeLayout xmlns:android=&quot;http://schemas.android.com/apk/res/android&quot; android:id=&quot;@+id/parent_layout&quot; android:layout_width=&quot;match_parent&quot; android:layout_height=&quot;match_parent&quot;&gt; &lt;android.support.v7.widget.CardView android:id=&quot;@+id/cardview&quot; android:layout_width=&quot;match_parent&quot; android:layout_height=&quot;wrap_content&quot; android:layout_margin=&quot;5dp&quot; card_view:cardCornerRadius=&quot;5dp&quot; xmlns:android=&quot;http://schemas.android.com/apk/res/android&quot; xmlns:card_view=&quot;http://schemas.android.com/apk/res-auto&quot;&gt; &lt;LinearLayout android:layout_width=&quot;match_parent&quot; android:layout_height=&quot;wrap_content&quot; android:orientation=&quot;vertical&quot;&gt; &lt;ImageView android:id=&quot;@+id/image&quot; android:layout_width=&quot;match_parent&quot; android:layout_height=&quot;210dp&quot; /&gt; &lt;TextView android:id=&quot;@+id/title&quot; android:layout_width=&quot;match_parent&quot; android:layout_height=&quot;wrap_content&quot; android:padding=&quot;10dp&quot; android:text=&quot;test&quot; /&gt; &lt;/LinearLayout&gt; &lt;/android.support.v7.widget.CardView&gt; &lt;/RelativeLayout&gt;
</pre>

1. CardView를 보여줄 Fragment 클래스를 만든다. onCreateView에서 recycle_fragment_layout을 inflate한다. 
<pre class="prettyprint">
class CardViewFragment : Fragment() {
    override fun onCreateView(
            inflater: LayoutInflater, container: ViewGroup?,
            savedInstanceState: Bundle?): View? {
        val view = inflater.inflate(R.layout.recycle_fragment_layout, container, false)
        
        val layoutManager = LinearLayoutManager(view.context)
        layoutManager.orientation = LinearLayoutManager.HORIZONTAL
        view.recyclerview.layoutManager = layoutManager

        items.add(R.drawable.ic_launcher, "title1"))
        items.add(Item(R.drawable.ic_launcher, "title2"))
        items.add(Item(R.drawable.ic_launcher, "title3"))

        view.recyclerview.adapter = RecyclerAdapter(view.context, items)
        return view
    }
}
</pre>

4. cardView에 들어갈 데이터인 Item 클래스를 만든다. 사진과 타이틀이 들어갈 것이다.  
<pre class="prettyprint">
class Item(private var image: Int, private var title: String) {

    fun getImage(): Int {
        return this.image
    }

    fun getTitle(): String {
        return this.title
    }
}
</pre>

5. RecycleView의 Adapter를 정의해 준다. 
<pre class="prettyprint">
class RecyclerAdapter : RecyclerView.Adapter<RecyclerAdapter.ViewHolder> {

    private lateinit var context: Context
    private var items = ArrayList<Item>()
    private var item_layout = 0

    constructor(context: Context, items: ArrayList<Item>, item_layout: Int) : super() {
        this.context = context
        this.items = items
        this.item_layout = item_layout
    }

    override fun onCreateViewHolder(parent: ViewGroup, item_layout: Int): RecyclerAdapter.ViewHolder {
        val v = LayoutInflater.from(parent.context).inflate(R.layout.item_cardview, null)
        return ViewHolder(v)
    }

    override fun getItemCount(): Int {
        return this.items.size
    }

    override fun onBindViewHolder(holder: RecyclerAdapter.ViewHolder, position: Int) {
        val item = items[position]
        val drawable = ContextCompat.getDrawable(context, item.getImage())
        holder.image!!.background = drawable
        holder.title!!.text = item.getTitle()
    }

    class ViewHolder : RecyclerView.ViewHolder {

        var image: ImageView? = null
        var title: TextView? = null
        var cardview: CardView? = null

        constructor(itemView: View) : super(itemView) {
            image = itemView.findViewById<View>(R.id.image) as ImageView
            title = itemView.findViewById<View>(R.id.title) as TextView
            cardview = itemView.findViewById<View>(R.id.cardview) as CardView
        }
    }
}
</pre>