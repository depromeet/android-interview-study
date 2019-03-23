# 안드로이드 개발자 면접 질문리스트

*질문에 대한 난이도는 상/중상/중/중하/하 로 나누었습니다.*

### RecyclerView
- 아이템 100개 와 ViewHolder 1개를 가진 RecyclerView의 동작원리를 설명 해주세요. **(중)**
- RecyclerView or ListView 의 Pagination 구현 방법을 설명 해주세요. **(중상)**
- 네트워크 통신을 통해 이미지를 가져오는 뷰가 포함된 ListView 또는 RecyclerView에서 빠르게 스크롤 시 생길 수 있는 이슈가 무엇이고 어떻게 수정 및 최적화를 할 수 있을까요? **(상)**

### Kotlin
- with, run 의 차이점은 무엇일까요? **(하)**
- run, let 의 차이점은 무엇일까요? **(하)**
- let, also 의 차이점은 무엇일까요? **(하)**
- Immutable 변수와 Mutable 변수를 쓰면 좋은점은 무엇일까요? **(중)**

### Rx
- 안드로이드에서 RxJava2 메모리 관리 하는 법은 무엇일까요? **(상)**

### 기타
- Parcel 과 Serializable의 차이는 무엇일까요? **(중)**
- 안드로이드에서 Unit Test가 필요 한 이유는 무엇일까요? **(중상)**
- 기존 프로젝트에서 “개발 서버를 바라보는 어플” 과 “프로덕션 서버를 바라보는 어플”을 나눠서 관리해야 한다고 했을 때 본인의 계획을 말씀 해주세요. **(하)**
- 인플레이션(inflation)이란 무엇인가요? **(중)**
- Intent를 통해 데이터 전달하는 과정에서 클래스 객체를 바로 전달하지 못하는 이유는 무엇이고 전달하기 위해서는 어떤 처리가 필요한가요? 그리고 Activity 간 데이터 전달을 위해 Intent 방법을 사용하는 이유가 무엇인가요? **(중상)**
- 복수의 Fragment 간 데이터 전달 방법을 설명 해주세요. **(중하)**
- Width가 1000px Height가 20000px인 이미지가 있고 해당 이미지를 보여주려고 했을 때 아래와 같은 에러가 떴다. 이를 이미지 라이브러리를 사용하지 않고 해결하는 방법에 대해 설명 해주세요. **(중)**
```
W/OpenGLRenderer: Bitmap too large to be uploaded into a texture (max=2048x2048)
```

### 코드 포함 질문

- 아래 코드에 주석을 보고 답을 해주세요.(1) **(상)**

```Kotlin
class LookpinTestItemHolder(view: View) : androidx.recyclerview.widget.RecyclerView.ViewHolder(view) {
    private val tvTitle: TextView = itemView.tv_title
    private val tvMessage: TextView = itemView.tv_message
    /** 
     * Adapter에는 100개의 똑같은 Data가 들어있고 count 는 100이다.
     * 그럼 ItemHolder에서 아주 미세 하더라도 성능을 개선할 수 있는 부분과 그 이유는?
     */
    fun bind(data: Data) {
        tvTitle.setMaxLines(data.maxLines)
        tvTitle.text = data.title
        tvMessage.setTextColor(ContextCompat.getColor(itemView.context, R.color.message_gray_10)
    }
}
```

아래 코드에 주석을 보고 답을 해주세요.(2) **(상)**

```Kotlin
data class Data(val veryLongMessage:String = "") : Parcelable { /**/ }

class LookpinTestItemHolder(view: View) : androidx.recyclerview.widget.RecyclerView.ViewHolder(view) {
    private val btnOk: Button = itemView.btn_ok
    /**
     * 해당 코드에서 Data는 매우 긴 메세지를 담고있다.
     * Crash가 발생한다면 어떤 상황 이였기에 Crash가 발생했는지 설명하고
     * 그리고 해결 방법은 어떻게 되는가?
     */
    fun bind(data: Data) {
        btnOk.setOnClickListener { _ ->
            Intent(itemView.context, ItemDetailActivity::class.java)
            .apply { putExtra("data", data) }
            .let { itemView.context.startActivity(it) }
        }
    }
}


class ItemDetailActivity : BaseActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.item_detail_activity)
        initData()
    }

    private fun initData() {
        val data = getIntent().getParcelableExtra<Data>("data")
        Log.d("test", data.veryLongMessage)
    }
}
```


- 아래 코드에 주석을 보고 답을 해주세요.(3) **(중)**

```Kotlin
class LookpinTestItemHolder(view: View) : androidx.recyclerview.widget.RecyclerView.ViewHolder(view) {
    private val ivPhoto: ImageView = itemView.ivPhoto
	
    fun bind(data: Data) {
        /** 
         * data.photoUrl은 이미지는 매번 다르지만 파일이름은 같음.
         * ImageLoadingLibrary의 DiskCache는 LRU 알고리즘으로 구현되어있고 Key는 URL을 암호화한 값입니다.
         * 그렇다면 여기서 생기는 문제와 해결방법은?
         */ 
        ImageLoadingLibrary.get(data.photoUrl).diskCacheStrategy(AUTO).into(ivPhoto)
    }
}
```

- 아래 코드에 주석을 보고 답을 해주세요.(4) **(중)**

```Kotlin
class LookpinTestItemHolder(view: View) : androidx.recyclerview.widget.RecyclerView.ViewHolder(view) {
    fun bind(data: Data) {
        /** 
         * 코틀린 안드로이드 익스텐션을 사용할 때
         * 아래와 같이 뷰 홀더 내부에서 직접 사용하면 성능적으로 문제가 되는 점은 무엇일까요?
         */ 
        itemView.tv_message.text = data.message
        itemView.tv_title.text = data.title
    }
}
```

- 아래 코드에 주석을 보고 답을 해주세요.(5) **(중)**

```Kotlin
class LookpinTestItemHolder(view: View) : androidx.recyclerview.widget.RecyclerView.ViewHolder(view) {
    fun bind(data: Data) {
        itemView.btn_like.isSelected = data.isLiked
        itemView.btn_link.setOnClickListener { _ -> 
            /** 
             * ItemHolder 안 입니다.
             * 좋아요 버튼을 누르면 서버에 요청(좋아요 취소 or 좋아요)을 날리고 응답이 오면
             * 해당 아이템의 UI가 업데이트가 되어야한다면 해당 Flow를 Recyclerview, Adapter, ItemHolder를 통해서 설명해주세요.
             */
        }
    }
}
```


---
#### 출처
 - [룩핀 안드로이드 개발 면접 리스트](https://medium.com/lookpin-engineering/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EA%B0%9C%EB%B0%9C%EC%9E%90-%EB%A9%B4%EC%A0%91-%EC%A7%88%EB%AC%B8%EB%A6%AC%EC%8A%A4%ED%8A%B8-63e1de17453b?fbclid=IwAR2eVKry5rTKADIdIIrVD6yBvcIlIfN2cddOYi9S6CE_iGw9YHFCnp6yikw)

