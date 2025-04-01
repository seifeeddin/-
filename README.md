import { useState, useEffect } from 'react';
import { db, auth } from '../firebase';
import { collection, addDoc, getDocs } from 'firebase/firestore';
import { signInWithPopup, GoogleAuthProvider, signOut } from 'firebase/auth';

export default function Home() {
  const [storeName, setStoreName] = useState('');
  const [product, setProduct] = useState('');
  const [price, setPrice] = useState('');
  const [products, setProducts] = useState([]);
  const [user, setUser] = useState(null);

  useEffect(() => {
    const fetchProducts = async () => {
      const querySnapshot = await getDocs(collection(db, 'stores'));
      setProducts(querySnapshot.docs.map(doc => ({ id: doc.id, ...doc.data() })));
    };
    fetchProducts();
  }, []);

  const addProduct = async () => {
    if (!user) return alert('يجب تسجيل الدخول لإضافة منتج');
    if (!storeName || !product || !price) return alert('أدخل جميع البيانات');
    
    try {
      await addDoc(collection(db, 'stores'), {
        storeName,
        product,
        price: parseFloat(price),
        createdAt: new Date(),
        user: user.displayName
      });
      alert('تمت إضافة المنتج!');
      setStoreName('');
      setProduct('');
      setPrice('');
    } catch (error) {
      console.error('خطأ في الإضافة:', error);
      alert('حدث خطأ');
    }
  };

  const login = async () => {
    const provider = new GoogleAuthProvider();
    try {
      const result = await signInWithPopup(auth, provider);
      setUser(result.user);
    } catch (error) {
      console.error('خطأ في تسجيل الدخول:', error);
    }
  };

  const logout = async () => {
    await signOut(auth);
    setUser(null);
  };

  return (
    <div className="p-6 max-w-md mx-auto bg-white rounded-xl shadow-md space-y-4">
      {user ? (
        <div>
          <p>مرحبًا، {user.displayName}</p>
          <button onClick={logout} className="bg-red-500 text-white p-2 rounded">تسجيل الخروج</button>
        </div>
      ) : (
        <button onClick={login} className="bg-blue-500 text-white p-2 rounded">تسجيل الدخول عبر Google</button>
      )}

      <h1 className="text-xl font-bold">إضافة منتج جديد</h1>
      <input type="text" placeholder="اسم المحل" value={storeName} onChange={e => setStoreName(e.target.value)} className="w-full p-2 border rounded" />
      <input type="text" placeholder="اسم المنتج" value={product} onChange={e => setProduct(e.target.value)} className="w-full p-2 border rounded" />
      <input type="number" placeholder="السعر" value={price} onChange={e => setPrice(e.target.value)} className="w-full p-2 border rounded" />
      <button onClick={addProduct} className="w-full bg-blue-500 text-white p-2 rounded">إضافة</button>
      <h2 className="text-lg font-bold mt-4">قائمة المنتجات</h2>
      <ul>
        {products.map(prod => (
          <li key={prod.id} className="border-b p-2">{prod.storeName} - {prod.product} - {prod.price} أوقية</li>
        ))}
      </ul>
    </div>
  );
}import { useState, useEffect } from 'react';
import { db, auth } from '../firebase';
import { collection, addDoc, getDocs } from 'firebase/firestore';
import { signInWithPopup, GoogleAuthProvider, signOut } from 'firebase/auth';

export default function Home() {
  const [storeName, setStoreName] = useState('');
  const [product, setProduct] = useState('');
  const [price, setPrice] = useState('');
  const [products, setProducts] = useState([]);
  const [user, setUser] = useState(null);

  useEffect(() => {
    const fetchProducts = async () => {
      const querySnapshot = await getDocs(collection(db, 'stores'));
      setProducts(querySnapshot.docs.map(doc => ({ id: doc.id, ...doc.data() })));
    };
    fetchProducts();
  }, []);

  const addProduct = async () => {
    if (!user) return alert('يجب تسجيل الدخول لإضافة منتج');
    if (!storeName || !product || !price) return alert('أدخل جميع البيانات');
    
    try {
      await addDoc(collection(db, 'stores'), {
        storeName,
        product,
        price: parseFloat(price),
        createdAt: new Date(),
        user: user.displayName
      });
      alert('تمت إضافة المنتج!');
      setStoreName('');
      setProduct('');
      setPrice('');
    } catch (error) {
      console.error('خطأ في الإضافة:', error);
      alert('حدث خطأ');
    }
  };

  const login = async () => {
    const provider = new GoogleAuthProvider();
    try {
      const result = await signInWithPopup(auth, provider);
      setUser(result.user);
    } catch (error) {
      console.error('خطأ في تسجيل الدخول:', error);
    }
  };

  const logout = async () => {
    await signOut(auth);
    setUser(null);
  };

  return (
    <div className="p-6 max-w-md mx-auto bg-white rounded-xl shadow-md space-y-4">
      {user ? (
        <div>
          <p>مرحبًا، {user.displayName}</p>
          <button onClick={logout} className="bg-red-500 text-white p-2 rounded">تسجيل الخروج</button>
        </div>
      ) : (
        <button onClick={login} className="bg-blue-500 text-white p-2 rounded">تسجيل الدخول عبر Google</button>
      )}

      <h1 className="text-xl font-bold">إضافة منتج جديد</h1>
      <input type="text" placeholder="اسم المحل" value={storeName} onChange={e => setStoreName(e.target.value)} className="w-full p-2 border rounded" />
      <input type="text" placeholder="اسم المنتج" value={product} onChange={e => setProduct(e.target.value)} className="w-full p-2 border rounded" />
      <input type="number" placeholder="السعر" value={price} onChange={e => setPrice(e.target.value)} className="w-full p-2 border rounded" />
      <button onClick={addProduct} className="w-full bg-blue-500 text-white p-2 rounded">إضافة</button>
      <h2 className="text-lg font-bold mt-4">قائمة المنتجات</h2>
      <ul>
        {products.map(prod => (
          <li key={prod.id} className="border-b p-2">{prod.storeName} - {prod.product} - {prod.price} أوقية</li>
        ))}
      </ul>
    </div>
  );
}
