# Cart

    public class CartBean extends HashMap{
    public void addSanPham(CartSP sp) {
        int key = sp.getSp().getId();
        if (this.containsKey(key)) {
            int sl = ((CartSP)this.get(key)).getSoluong();
            ((CartSP)this.get(key)).setSoluong(sl+1);
        }else{
            this.put(sp.getSp().getId(), sp);
        }
    }
    public void addSP(CartSP sp, int Sol) {
        int key = sp.getSp().getId();
        if (this.containsKey(key)) {
            int sl = ((CartSP)this.get(key)).getSoluong();
            ((CartSP)this.get(key)).setSoluong(Sol);
        }else{
            this.put(sp.getSp().getId(), sp);
        }
    }
    
    public boolean removeSanPham(int code) {
        if(this.containsKey(code)) {
            this.remove(code);
            return true;
        }
        return false;
    }
    public CartBean() {
        super();
    }
    }
    
 --- CartSP
    
    public class CartSP implements Serializable {

    private Sanpham sp;
    private int soluong;

    public CartSP() {
    }

    public CartSP(Sanpham sp) {
        this.sp = sp;
        this.soluong = 1;
    }

    public CartSP(Sanpham sp, int soluong) {
        this.sp = sp;
        this.soluong = soluong;
    }

    public Sanpham getSp() {
        return sp;
    }

    public void setSp(Sanpham sp) {
        this.sp = sp;
    }

    public int getSoluong() {
        return soluong;
    }

    public void setSoluong(int soluong) {
        this.soluong = soluong;
    }

    }
    
