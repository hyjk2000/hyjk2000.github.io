---
title: MySQL Syntax：三个表的左连接查询
author: James Shih
layout: post
permalink: /2010/03/30/mysql-syntax%ef%bc%9a%e4%b8%89%e4%b8%aa%e8%a1%a8%e7%9a%84%e5%b7%a6%e8%bf%9e%e6%8e%a5%e6%9f%a5%e8%af%a2/
spaces_666edbae79c1d30694f5e5d213cfdf0f_permalink:
  - "http://cid-6ed4d3e2ba61f4c5.users.api.live.net/Users(7986241009977783493)/Blogs('6ED4D3E2BA61F4C5!102')/Entries('6ED4D3E2BA61F4C5!1273')?authkey=72j5ZQnBJYQ%24"
  - "http://cid-6ed4d3e2ba61f4c5.users.api.live.net/Users(7986241009977783493)/Blogs('6ED4D3E2BA61F4C5!102')/Entries('6ED4D3E2BA61F4C5!1273')?authkey=72j5ZQnBJYQ%24"
categories:
  - SQL
  - Web 开发
---
<div id="msgcns!6ED4D3E2BA61F4C5!1273" class="bvMsg">
  <p>
    设有以下3个表，我们可以看出，Jim是Website Develop部门的Programmer；Harry是Website Develop部门的Designer；而Wills是总经理，不属于任何部门。
  </p>
  
  <p>
    现在要做的是：列出所有用户，以及他们所属的部门和职位。
  </p>
  
  <p>
    tb_user 用户<br /> <table border="1" cellspacing="0" cellpadding="2" width="299">
      <tr>
        <td valign="top" width="100">
          <strong>id</strong>
        </td>
        
        <td valign="top" width="100">
          <strong>sName</strong>
        </td>
        
        <td valign="top" width="100">
          <strong>id_Pos</strong>
        </td>
      </tr>
      
      <tr>
        <td valign="top" width="100">
          1
        </td>
        
        <td valign="top" width="100">
          Jim
        </td>
        
        <td valign="top" width="100">
          2
        </td>
      </tr>
      
      <tr>
        <td valign="top" width="100">
          2
        </td>
        
        <td valign="top" width="100">
          Harry
        </td>
        
        <td valign="top" width="100">
          3
        </td>
      </tr>
      
      <tr>
        <td valign="top" width="100">
          3
        </td>
        
        <td valign="top" width="100">
          Wills
        </td>
        
        <td valign="top" width="100">
          1
        </td>
      </tr>
    </table>
    
    <p>
      tb_pos 职位<br /> <table border="1" cellspacing="0" cellpadding="2" width="292">
        <tr>
          <td valign="top" width="100">
            <strong>id</strong>
          </td>
          
          <td valign="top" width="100">
            <strong>sCaption</strong>
          </td>
          
          <td valign="top" width="100">
            <strong>id_Dpt</strong>
          </td>
        </tr>
        
        <tr>
          <td valign="top" width="100">
            1
          </td>
          
          <td valign="top" width="100">
            Manager
          </td>
          
          <td valign="top" width="100">
          </td>
        </tr>
        
        <tr>
          <td valign="top" width="100">
            2
          </td>
          
          <td valign="top" width="100">
            Programmer
          </td>
          
          <td valign="top" width="100">
            1
          </td>
        </tr>
        
        <tr>
          <td valign="top" width="100">
            3
          </td>
          
          <td valign="top" width="100">
            Designer
          </td>
          
          <td valign="top" width="100">
            1
          </td>
        </tr>
      </table>
      
      <p>
        tb_dpt 部门<br /> <table border="1" cellspacing="0" cellpadding="2" width="290">
          <tr>
            <td valign="top" width="100">
              <strong>id</strong>
            </td>
            
            <td valign="top" width="200">
              <strong>sCaption</strong>
            </td>
          </tr>
          
          <tr>
            <td valign="top" width="100">
              1
            </td>
            
            <td valign="top" width="200">
              WebSite Develop
            </td>
          </tr>
          
          <tr>
            <td valign="top" width="100">
              2
            </td>
            
            <td valign="top" width="200">
              Server Management
            </td>
          </tr>
        </table>
        
        <p>
          由于要列出tb_user中的每一条记录，这里我们需要用左连接查询。而这里要连接3个表，所以应该这样写：
        </p>
        
        <blockquote>
          <p>
            <font face="Courier New">SELECT u.sName p.sCaption d.sCaption FROM <font color="#00ff00">tb_user AS u LEFT JOIN</font> (<font color="#ffff00">tb_pos AS p LEFT JOIN tb_dpt AS d ON p.id_Dpt=d.id</font>) <font color="#00ff00">ON u.id_Pos=p.id</font>;</font>
          </p>
        </blockquote>
        
        <p>
          先连接了tb_pos和tb_dpt形成一个新表，然后再让tb_user去连接这个新表。
        </p>
      </p></div>