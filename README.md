# Windows Server Network Load Balancing
<div>Web serverlarımıza gelen istekleri dengelemek ve daha fazla isteğe web uygulamamızın cevap verebilmesini istediğinizde bir çok donanım kullanılarak load balancing&nbsp; işlemi uygulayabilirsiniz ancak finansal süreçler ve network alt yapınız kapsamında yeni
        bir yatrıım durumu söz konusu değil ise Windows Server Network Load Balancing (NLB) feature ile bu süreçi gerçekleştirebilirsiniz.
        <br>
        </div>
        <div><br>
        </div>
        <div>Bu yapıda ihtiyacınız olacak bire bir aynı şekilde kurulmuş ve yapılandırılmış 2 adet IIS sunucusudur. IIS sunucularımızın üzerine NLB feature kurulumu yaparak yapılandırma işlemlerini tamamlayıp load balancing sürecinizi gerçekleştirebilirsiniz.
        <br>
        </div>
        <div><br>
        </div>
        <div>Bu makalemde sizlere 2 adet IIS sunucusuna NLB feature kurulumu yaparak load balancing işlemini nasıl yapacağınızı anlatacağım.
        <br>
        </div>
        <div><br>
        </div>
        <div>İhtiyacınız olanlar. <br>
        </div>
        <div>
        <ul>
            <li>2 adet Windows Server 2019 işletim sistemi yüklü sunucu (Windows server eski sürümlerinde de bu işlemi gerçekleştirebilirsiniz. makale sürecinde ben Windows Server 2019 kullanıyor olacağım.)
            </li>
            <li>2 sunucu üzerine web server role and featurelarının kurulmuş olması gerekmektedir.
            </li>
            <li>2 sunucu üzerine network load balancing (NLB) feature kurulumuş olması gerekmektedir.
            </li>
        </ul>
        </div>
        <div>Windows Server 2019 kurulumu sonrası network ayarlarınızı yapmış gerekli olacak driverları yüklemiş olmanız gerekemektedir. Bu adımları tamamladıysanız IIS kurulumunu aşağıdaki komut ile yapabilirsiniz.
        <br>
        </div>
        <div><br>
        </div>
        <div>
        <div class="reCodeBlock" style="border: 1px solid #7f9db9; overflow-y: auto;">
        <div style="background-color: #ffffff;"><span style="margin-left: 0px !important;"><code style="color: #000000;">PS C:\Users\Administrator&gt; Install-WindowsFeature Web-Server -IncludeManagementTools</code></span></div>
        <div style="background-color: #f8f8f8;"><span style="margin-left: 0px !important;">&nbsp;</span></div>
        <div style="background-color: #ffffff;"><span style="margin-left: 0px !important;"><code style="color: #000000;">Success Restart Needed Exit Code&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Feature Result</code></span></div>
        <div style="background-color: #f8f8f8;"><span style="margin-left: 0px !important;"><code style="color: #000000;">------- -------------- ---------&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; --------------</code></span></div>
        <div style="background-color: #ffffff;"><span style="margin-left: 0px !important;"><code style="color: #000000;">True&nbsp;&nbsp;&nbsp; No&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Success&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {Common HTTP Features, Default Document, D...</code></span></div>
        </div>
        </div>
        <div><br>
        </div>
        <div>IIS kurulumunuzu özelleştirmek ve kapsamlı bir yükleme yapmak için aşağıdaki linkleri inceleyebilirsiniz.
        <br>
        </div>
        <div><br>
        </div>
        <div>
        <div class="reCodeBlock" style="border: 1px solid #7f9db9; overflow-y: auto;">
        <div style="background-color: #ffffff;"><span style="margin-left: 0px !important;"><code style="color: #000000;"><a href="https://social.technet.microsoft.com/wiki/contents/articles/52833.powershell-ile-iis-kurulumu-tr-tr.aspx" target="blank">https://social.technet.microsoft.com/wiki/contents/articles/52833.powershell-ile-iis-kurulumu-tr-tr.aspx</a></code></span></div>
        <div style="background-color: #f8f8f8;"><span style="margin-left: 0px !important;"><code style="color: #000000;"><a href="https://social.technet.microsoft.com/wiki/contents/articles/52821.windows-server-gui-iis-kurulumu-tr-tr.aspx" target="blank">https://social.technet.microsoft.com/wiki/contents/articles/52821.windows-server-gui-iis-kurulumu-tr-tr.aspx</a></code></span></div>
        </div>
        </div>
        <div><br>
        </div>
        <div>Role and feature yüklemelerimizde ikinci adımda NLB featurenu kurmanız gerekmektedir.
        <br>
        </div>
        <div><br>
        </div>
        <div>
        <div class="reCodeBlock" style="border: 1px solid #7f9db9; overflow-y: auto;">
        <div style="background-color: #ffffff;"><span style="margin-left: 0px !important;"><code style="color: #000000;">PS C:\Users\Administrator&gt; Install-WindowsFeature NLB -IncludeManagementTools</code></span></div>
        <div style="background-color: #f8f8f8;"><span style="margin-left: 0px !important;">&nbsp;</span></div>
        <div style="background-color: #ffffff;"><span style="margin-left: 0px !important;"><code style="color: #000000;">Success Restart Needed Exit Code&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Feature Result</code></span></div>
        <div style="background-color: #f8f8f8;"><span style="margin-left: 0px !important;"><code style="color: #000000;">------- -------------- ---------&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; --------------</code></span></div>
        <div style="background-color: #ffffff;"><span style="margin-left: 0px !important;"><code style="color: #000000;">True&nbsp;&nbsp;&nbsp; No&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Success&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {Network Load Balancing, Remote Server Adm...</code></span></div>
        </div>
        </div>
        <div><br>
        </div>
        <div>Elimizde aşağıdaki şekilde iki sunucum bulunmaktadır.</div>
        <div><br>
        </div>
        <div>PowerShell-Ozan: IIS ve NLB yüklü</div>
        <div><br>
        </div>
        <div>
        <div class="reCodeBlock" style="border: 1px solid #7f9db9; overflow-y: auto;">
        <div style="background-color: #ffffff;"><span style="margin-left: 0px !important;"><code style="color: #000000;">[X] Web Server&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Web-WebServer&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Installed</code></span></div>
        <div style="background-color: #f8f8f8;"><span style="margin-left: 0px !important;"><code style="color: #000000;">[X] Network Load Balancing&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; NLB&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Installed</code></span></div>
        </div>
        <br>
        </div>
        <div>PowerShell2-Ozan: IIS ve NLB yüklü <br>
        </div>
        <div><br>
        </div>
        <div>
        <div class="reCodeBlock" style="border: 1px solid #7f9db9; overflow-y: auto;">
        <div style="background-color: #ffffff;"><span style="margin-left: 0px !important;"><code style="color: #000000;">[X] Web Server&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Web-WebServer&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Installed</code></span></div>
        <div style="background-color: #f8f8f8;"><span style="margin-left: 0px !important;"><code style="color: #000000;">[X] Network Load Balancing&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; NLB&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Installed</code></span></div>
        </div>
        <br>
        </div>
        <div>İki sunucunuz üzerinde IIS ve NLB kurulumunu tamamladıktan sonra test amaçlı IIS sunucularında demo web sitelerimi yüklüyorum.
        <br>
        </div>
        <div>IIS10.0&nbsp; varsayılan görselinde ufak bir editleme yaparak ben işlemime devam ediyorum örnek çalışmalar aşağdıaki gibidir.
        <br>
        </div>
        <div><br>
        </div>
        <div><a href="https://www.emreozanmemis.com/wp-content/uploads/2019/05/3.png" target="blank"><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/05/3.png" style="max-width: 550px; border-width: 0px; border-style: solid;"></a></div>
        <div><br>
        </div>
        <div><br>
        </div>
        <div>IIS üzerinde web sitemizide yapılandırdığımıza göre artık NLB yapılandırmasına geçebiliriz.
        <br>
        </div>
        <div><br>
        </div>
        <div>Network Load Balancing Manager uygulamasını açıyoruz.</div>
        <div><br>
        </div>
        <div>Sl üst bölümde bulunan Network Load Balancing Cluster sağ tıklayınız ve New Cluster seçiniz.
        <br>
        </div>
        <div><br>
        </div>
        <div><a href="https://www.emreozanmemis.com/wp-content/uploads/2019/05/NLBnewcluster.png" target="blank"><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/05/NLBnewcluster.png" style="max-width: 550px; border-width: 0px; border-style: solid;"></a><br>
        </div>
        <div><br>
        </div>
        <div>New Cluster penceresinde host bölümüne 1.IIS sunucunuzun hostname yada IP adresini giriniz.
        <br>
        </div>
        <div><br>
        </div>
        <div>Next butonuna basınız . <br>
        </div>
        <div><br>
        </div>
        <div><a href="https://www.emreozanmemis.com/wp-content/uploads/2019/05/NLBnewcluster1.png" target="blank"><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/05/NLBnewcluster1.png" style="max-width: 550px; border-width: 0px; border-style: solid;"></a></div>
        <div><br>
        </div>
        <div>Priotity bölümünün 1 olduğuna dikkat ediniz ilk IIS sunucumuz için 1 olması gerekmektedir.
        <br>
        </div>
        <div><br>
        </div>
        <div>Next butonuna basarak devam ediniz. <br>
        </div>
        <div><br>
        </div>
        <div><a href="https://www.emreozanmemis.com/wp-content/uploads/2019/05/NLBnewcluster2.png" target="blank"><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/05/NLBnewcluster2.png" style="max-width: 550px; border-width: 0px; border-style: solid;"></a></div>
        <div><br>
        </div>
        <div>CLusterimiz için load balancing yapacak olan IP adresimizi belirliyoruz. <br>
        </div>
        <div><br>
        </div>
        <div><a href="https://www.emreozanmemis.com/wp-content/uploads/2019/05/NLBnewcluster3.png" target="blank"><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/05/NLBnewcluster3.png" style="max-width: 550px; border-width: 0px; border-style: solid;"></a><br>
        </div>
        <div><br>
        </div>
        <div>Cluster parameters ayarlarımızı yapılandırıyoruz. <br>
        </div>
        <div><br>
        </div>
        <div><a href="https://www.emreozanmemis.com/wp-content/uploads/2019/05/NLBnewcluster4.png" target="blank"><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/05/NLBnewcluster4.png" style="max-width: 550px; border-width: 0px; border-style: solid;"></a></div>
        <div><br>
        </div>
        <div>Port rules adımında gelecek istekleri hangi porta yönlendireceğimizi belirliyoruz. daha fazla port ekleyerek yönlendirmelerinizi bu adımda gerçekleştirebilirsiniz.
        <br>
        </div>
        <div><br>
        </div>
        <div><a href="https://www.emreozanmemis.com/wp-content/uploads/2019/05/NLBnewcluster5.png" target="blank"><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/05/NLBnewcluster5.png" style="max-width: 550px; border-width: 0px; border-style: solid;"></a></div>
        <div><br>
        </div>
        <div>Clusterimiz üzerinde ilk IIS sunucu ekleme işlemimiz tamamlanmıştır. <br>
        </div>
        <div><br>
        </div>
        <div><a href="https://www.emreozanmemis.com/wp-content/uploads/2019/05/NLBnewcluster6.png" target="blank"><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/05/NLBnewcluster6.png" style="max-width: 550px; border-width: 0px; border-style: solid;"></a></div>
        <div><br>
        </div>
        <div>İkinci IIS sunucumuzu eklemek için&nbsp; sol üst bölümden add host to cluster butonuna basıyoruz.
        <br>
        </div>
        <div><br>
        </div>
        <div><a href="https://www.emreozanmemis.com/wp-content/uploads/2019/05/NLBnewcluster7-5.png" target="blank"><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/05/NLBnewcluster7-5.png" style="max-width: 550px; border-width: 0px; border-style: solid;"></a></div>
        <div><br>
        </div>
        <div>ikinci IIS sunucumuzun IP adresini giriyoruz. <br>
        </div>
        <div><br>
        </div>
        <div><a href="https://www.emreozanmemis.com/wp-content/uploads/2019/05/NLBnewcluster7-2.png" target="blank"><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/05/NLBnewcluster7-2.png" style="max-width: 550px; border-width: 0px; border-style: solid;"></a></div>
        <div><br>
        </div>
        <div><a href="https://www.emreozanmemis.com/wp-content/uploads/2019/05/NLBnewcluster7-3.png" target="blank"><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/05/NLBnewcluster7-3.png" style="max-width: 550px; border-width: 0px; border-style: solid;"></a></div>
        <div><br>
        </div>
        <div>Önemli olan bu bölümde port yönlendrimesin Equal opsiyonun seçilmiş olmasıdır.
        <br>
        </div>
        <div><br>
        </div>
        <div><a href="https://www.emreozanmemis.com/wp-content/uploads/2019/05/NLBnewcluster7-4.png" target="blank"><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/05/NLBnewcluster7-4.png" style="max-width: 550px; border-width: 0px; border-style: solid;"></a></div>
        <div><br>
        </div>
        <div>finish butonuna basark işlemlerimizi tamamlıyoruz. <br>
        </div>
        <div><br>
        </div>
        <div>Cluster üzerinde iki hostumuzu da artık görüyoruz. web sitemizi çağırdığımızda duruma istek artması durumında 2. host üzerinden görüntülenecektir. En başta yüklemesini gerçekleştirdiğimiz iki aynı görsel üzerindeki farklı hostları simgeleyen görsellerimizi
        göreceğiz. <br>
        </div>
        <div><br>
        </div>
        <div><a href="https://www.emreozanmemis.com/wp-content/uploads/2019/05/1IIS.png" target="blank"><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/05/1IIS.png" style="max-width: 550px; border-width: 0px; border-style: solid;"></a></div>
        <div><br>
        </div>
        <div><a href="https://www.emreozanmemis.com/wp-content/uploads/2019/05/2IIS.png" target="blank"><img alt="" src="https://www.emreozanmemis.com/wp-content/uploads/2019/05/2IIS.png" style="max-width: 550px; border-width: 0px; border-style: solid;"></a><br>
        </div>
        <div><br>
        </div>
