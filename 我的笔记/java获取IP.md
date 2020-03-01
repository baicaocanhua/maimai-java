private String getIpAddr() {   
     String ipAddress = null;   
     //ipAddress = this.getRequest().getRemoteAddr();   
     ipAddress = this.getRequest().getHeader("x-forwarded-for");   
     if(ipAddress == null || ipAddress.length() == 0 || "unknown".equalsIgnoreCase(ipAddress)) {   
      ipAddress = this.getRequest().getHeader("Proxy-Client-IP");   
     }   
     if(ipAddress == null || ipAddress.length() == 0 || "unknown".equalsIgnoreCase(ipAddress)) {   
         ipAddress = this.getRequest().getHeader("WL-Proxy-Client-IP");   
     }   
     if(ipAddress == null || ipAddress.length() == 0 || "unknown".equalsIgnoreCase(ipAddress)) {   
      ipAddress = this.getRequest().getRemoteAddr();   
      if(ipAddress.equals("127.0.0.1")){   
       //根据网卡取本机配置的IP   
       InetAddress inet=null;   
    try {   
     inet = InetAddress.getLocalHost();   
    } catch (UnknownHostException e) {   
     e.printStackTrace();   
    }   
    ipAddress= inet.getHostAddress();   
      }   
            

     }   
      
     //对于通过多个代理的情况，第一个IP为客户端真实IP,多个IP按照','分割   
     if(ipAddress!=null && ipAddress.length()>15){ //"***.***.***.***".length() = 15   
         if(ipAddress.indexOf(",")>0){   
             ipAddress = ipAddress.substring(0,ipAddress.indexOf(","));   
         }   
     }   
     return ipAddress;    
  }   

  public static String getIpAddress(HttpServletRequest request) {
    String ip = request.getHeader("x-forwarded-for");
    if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
        ip = request.getHeader("Proxy-Client-IP");
    }
    if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
        ip = request.getHeader("WL-Proxy-Client-IP");
    }
    if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
        ip = request.getRemoteAddr();
    }
    if (ip.contains(",")) {
        return ip.split(",")[0];
    } else {
        return ip;
    }
}

/** 
     * 获取用户真实IP地址，不使用request.getRemoteAddr()的原因是有可能用户使用了代理软件方式避免真实IP地址, 
          * 可是，如果通过了多级反向代理的话，X-Forwarded-For的值并不止一个，而是一串IP值 
          *   @return ip
               *     */
              private String getIpAddr(HttpServletRequest request) {
        String ip = request.getHeader("x-forwarded-for"); 
        System.out.println("x-forwarded-for ip: " + ip);
        if (ip != null && ip.length() != 0 && !"unknown".equalsIgnoreCase(ip)) {  
            // 多次反向代理后会有多个ip值，第一个ip才是真实ip
                if( ip.indexOf(",")!=-1 ){
            ip = ip.split(",")[0];
                    }
            }  
        if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {  
            ip = request.getHeader("Proxy-Client-IP");  
                System.out.println("Proxy-Client-IP ip: " + ip);
            }  
        if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {  
            ip = request.getHeader("WL-Proxy-Client-IP");  
                System.out.println("WL-Proxy-Client-IP ip: " + ip);
            }  
        if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {  
            ip = request.getHeader("HTTP_CLIENT_IP");  
                System.out.println("HTTP_CLIENT_IP ip: " + ip);
            }  
        if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {  
            ip = request.getHeader("HTTP_X_FORWARDED_FOR");  
                System.out.println("HTTP_X_FORWARDED_FOR ip: " + ip);
            }  
        if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {  
            ip = request.getHeader("X-Real-IP");  
                System.out.println("X-Real-IP ip: " + ip);
            }  
        if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {  
            ip = request.getRemoteAddr();  
                System.out.println("getRemoteAddr ip: " + ip);
            } 
        System.out.println("获取客户端ip: " + ip);
        return ip;  
            }
    
    ## [Java Web 获取客户端真实IP](https://www.cnblogs.com/xiaoxing/p/6565573.html)
    
* # 用Java来获取访问者真实的IP地址

* <https://blog.csdn.net/qq_36411874/article/details/79938439>