# Cart

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
