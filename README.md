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
    
    ---Add Sp
                HttpSession ss = request.getSession(true);
                Object check = ss.getValue("nameLogin");
                if (check.equals(null)) {
                    RequestDispatcher rd = request.getRequestDispatcher("dangnhap.jsp");
                    rd.forward(request, response);
                } else {
                    HttpSession session = request.getSession(true);
                    Cart shop = (Cart) session.getAttribute("SHOP");
                    if (shop == null) {
                        shop = new Cart();
                    }
                    String masp = request.getParameter("txtMaSP");
                    int ma = Integer.parseInt(masp);
                    String name = request.getParameter("txtTenSP");
                    String ttin = request.getParameter("txtTtinSP");
                    String gia = request.getParameter("txtGiaSP");
                    String sl = request.getParameter("txtSoSP");
                    String slm = request.getParameter("txtSlmSP");
                    int som = Integer.parseInt(slm);
                    String anh = request.getParameter("txtAnhSP");
                    int so = Integer.parseInt(sl);
                    if (Integer.parseInt(sl) < 0) {
                        RequestDispatcher rd = request.getRequestDispatcher("sanphamchitiet.jsp");
                        rd.forward(request, response);
                        return;
                    }
                    Sanpham s = new Sanpham(ma, name, ttin, gia, sl, anh);
                    CartSP sp = new CartSP(s, som);
                    cart.add(sp);
                    shop.addSanpham(sp, som);
                    session.setAttribute("SHOP", shop);
                    RequestDispatcher rd = request.getRequestDispatcher("giohang.jsp");
                    rd.forward(request, response);
                                    
                    //Lưu giỏ hàng
                    cartlist.clear();
                    for (int i = 0; i < shop.size(); i++) {
                        CartSP spp = cart.get(i);
                        int a = spp.getSp().getMasp();
                        String b = spp.getSp().getTen();
                        String c = spp.getSp().getTtin();
                        String d = spp.getSp().getGia();
                        String e = spp.getSp().getSoluong();
                        String f = String.valueOf(spp.getSoluong());
                        String g = spp.getSp().getAnh();
                        System.out.println(a + "," + b + "," + c + "," + d + "," + e + "," + f + "," + g);
                        Giohang gh = new Giohang(a, b, c, d, e, f, g);
                        cartlist.add(gh);
                    }
                    Gson gson = new Gson();
                    String json = gson.toJson(cartlist);
                    LoginBean login = new LoginBean();
                    if (!username.equals("")) {
                        login.LuuGH(username, json);
                    }
                }
                
    --- Xóa Sp
        String sp = request.getParameter("txtMaspGH");
                HttpSession session = request.getSession(true);
                Cart shop = (Cart) session.getAttribute("SHOP");
                if (session != null) {
                    if (shop != null) {
                        shop.removeSanpham(Integer.parseInt(sp));
                        session.setAttribute("SHOP", shop);
                        for (int i = 0; i < cart.size(); i++) {
                            System.out.println(cart.get(i).getSp().getMasp());
                            if (cart.get(i).getSp().getMasp() == Integer.parseInt(sp)) {
                                cart.remove(i);
                            }
                        }
                        for (int i = 0; i < cartlist.size(); i++) {
                            if (cartlist.get(i).getMasp() == Integer.parseInt(sp)) {
                                cartlist.remove(i);
                            }
                        }

                    }
                }
                RequestDispatcher rd = request.getRequestDispatcher("giohang.jsp");
                rd.forward(request, response);
                //Lưu giỏ hàng
                cartlist.clear();
                for (int i = 0; i < shop.size(); i++) {
                    CartSP spp = cart.get(i);
                    int a = spp.getSp().getMasp();
                    String b = spp.getSp().getTen();
                    String c = spp.getSp().getTtin();
                    String d = spp.getSp().getGia();
                    String e = spp.getSp().getSoluong();
                    String f = String.valueOf(spp.getSoluong());
                    String g = spp.getSp().getAnh();
                    Giohang gh = new Giohang(a, b, c, d, e, f, g);
                    cartlist.add(gh);
                }
                Gson gson = new Gson();
                String json = gson.toJson(cartlist);
                LoginBean login = new LoginBean();
                if (!username.equals("")) {
                    login.LuuGH(username, json);
                }
    --- Show SP
    <c:set var="shop" value="${sessionScope.SHOP}"/>
                            <c:if test="${not empty shop}">
                                <tbody>
                                    <c:set var="count" value="0"/>
                                    <c:forEach var="rows" items="${shop}">
                                        <c:set var="count" value="${count + 1}" />
                                    <form action="Controller" method="post">
                                        <tr>
                                            <td class="checkbox"><input type="checkbox" class="w3-check" name="rmv" value="${rows.value.sp.masp}" onChange="setPrice()"/></td>
                                            <td class="img-cart"><img src="images/${rows.value.sp.anh}" alt="" name="img-cart" class="w3-round"/></td>
                                            <td class="label-name"><label style="padding-left: 10px;">${rows.value.sp.ten}</label></td>
                                            <td>
                                                <button class="w3-button w3-gray w3-round-large" onClick="setindex('-', this)">-</button>
                                                <input class="input-soluong" type="number" min="1" max="${rows.value.sp.soluong}" value="${rows.value.soluong}" name="txtslm${rows.value.sp.masp}" onBlur="loadindex(this)"/>
                                                <button class="w3-button w3-gray w3-round-large" onClick="setindex('+', this)">+</button><br>
                                            </td>
                                            <td><span>${rows.value.sp.gia}</span> VND</td>
                                            <td class="w3-text-red"><span></span> VND</td>
                                        <input type="hidden" value="${rows.value.sp.masp}" name="txtMaspGH"/>
                                        <td style="text-align: center"><button type="submit" name="btnAction" value="RemoveCart" class="w3-button"><img src="images/iconDelete.png" alt=""/></button></td>
                                        </tr>
                                    </form>
                                </c:forEach>
                                </tbody>
                            </c:if>
    ---Login Giohang
                Gson gson = new Gson();
                cartlist = gson.fromJson(tv.getGiohang(), new TypeToken<List<Giohang>>() {
                }.getType());
                HttpSession session = request.getSession(true);
                Cart shop = (Cart) session.getAttribute("SHOP");
                if (shop == null) {
                    shop = new Cart();
                }
                for (Giohang cat : cartlist) {
                    int a = cat.getMasp();
                    String b = cat.getTen();
                    String c = cat.getTtin();
                    String d = cat.getGia();
                    String e = cat.getSoluong();
                    String f = cat.getSoluongmua();
                    String g = cat.getAnh();
                    Sanpham s = new Sanpham(a, b, c, d, e, g);
                    CartSP sp = new CartSP(s, Integer.parseInt(f));
                    cart.add(sp);
                    shop.addSanpham(sp, Integer.parseInt(f));
                }

                session.setAttribute("SHOP", shop);
