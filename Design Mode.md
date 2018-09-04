# C++实现线程安全的单例模式（Singleton）
在某些应用环境下面，一个类只允许有一个实例，这就是著名的单例模式。

单例模式范围饿汉模式和懒汉模式两种。

## 饿汉模式实现

	#include <memory>
	template <class T>
	class singleton
	{
		protected:
			singleton(){};
		privated:
			singleton(const singleton&){}; // 禁止拷贝
			singleton& operator=(const singleton&); // 禁止赋值
			// 使用auto_ptr能够避免内存泄漏问题
			static auto_ptr<T> m_instance;
		public:
			// GetInstance声明成static的原因有两点
			// 1. 通过类名就能访问，因为构造函数为privated，无法通过创建实例，也就无法通过实例调用
			// 2. 能够访问static成员变量
			static auto_ptr<T> GetInstance();
	};

	template <class T>
	auto_ptr<T> singleton<T>::GetInstance(){return m_instance;}
	
	template <class T>
	auto_ptr<T> singleton<T>::m_instance = new T();

解释：

在实例化`m_instance`变量时，直接调用类的构造函数。也就是说，在还未使用变量的时候，就已经对`m_instance`进行赋值，就像很饥饿一样。这种模式，在多线程环境下肯定是安全的。因为无法调用构造函数，即不能通过代码创建singleton的实例，只能使用类变量`m_instance`，无论在哪个线程中，通过`singleton.GetInstance()`获取到的都是同一个实例，因此不可能存在多线程实例化的问题。

饿汉模式需要注意的点：

1. 构造函数定义成`protected`或`private`，保证无法通过构造函数创建`singleton`的实例。
2. 只允许一个实例的类定义成`singleton`类的`private static`指针。
3. `m_instance`定义成`auto_ptr`以避免内存泄漏。
4. 提供`public`方法供外界获取`m_instance`。
5. 由于`m_instance`是静态成员变量，因此必须在类外（**在.cpp文件中**）初始化。

提示：

1. 类T可以有多个实例，这里的单例指的是通过类`singleton`获取到的实例只可能有一个。
2. 由于成员变量`m_instance`为指针，为了避免内存泄漏问题，使用了`auto_ptr`。

## 懒汉模式实现

	#include <memory>
	template <class T>
	class singleton
	{
		protected:
			singleton(){};
		privated:
			singleton(const singleton&){}; // 禁止拷贝
			singleton& operator=(const singleton&); // 禁止赋值
			// 使用auto_ptr能够避免内存泄漏问题
			static auto_ptr<T> m_instance;
		public:
			// GetInstance声明成static的原因有两点
			// 1. 通过类名就能访问，因为构造函数为privated，无法通过创建实例，也就无法通过实例调用
			// 2. 能够访问static成员变量
			static auto_ptr<T> GetInstance();
	}
	
	template <class T>
	T* singleton<T>::GetInstance(){
		if(m_instance == nullptr)
		{
			// 注意，此处不能写成auto_ptr<T> m_instance(new T)
			// 因为有可能在实例没有创建完时，m_instance就不为nullptr了，这时另外一个线程在使用m_instance时会产生无法预料的行为。
			// 所以此处使用了中间变量，保证了线程安全。另外赋值操作是原子性的，因此使用中间变量之后，线程安全。
			auto_ptr<T> temp(new T);
			m_instance = temp;
		}
		return m_instance;
	}
	
	template <class T>
	T* singleton<T>::m_instance = nullptr;