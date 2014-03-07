---
title: .net密码加密代码
description: .net密码加密代码
keywords: code
layout: post
tags: [代码]
---
<pre class="code">

Code highlighting produced by Actipro CodeHighlighter (freeware)http://www.CodeHighlighter.com/-->using System;
using System.Data;
using System.Configuration;
using System.Web;
using System.Web.Security;
using System.Web.UI;
using System.Web.UI.WebControls;
using System.Web.UI.WebControls.WebParts;
using System.Web.UI.HtmlControls;
using System.Security.Cryptography;
using System.Text;

public class EncryptPassWord
{
    /// <summary>
    /// 获取密钥
    /// </summary>
    /// <returns></returns>
    public static string CreateSalt()
    {
        byte[] data = new byte[8];
        new RNGCryptoServiceProvider().GetBytes(data);
        return Convert.ToBase64String(data);
    }

    /// <summary>
    /// 加密密码
    /// </summary>
    /// <param name="pwdString"></param>
    /// <param name="salt"></param>
    /// <returns></returns>
    public static string EncryptPwd(string pwdString, string salt)
    {
        if (salt == null || salt == "")
        {
            return pwdString;
        }
        byte[] bytes = Encoding.Unicode.GetBytes(salt.ToLower().Trim() + pwdString.Trim());
        return BitConverter.ToString(((HashAlgorithm)CryptoConfig.CreateFromName("SHA1")).ComputeHash(bytes));
    }
}
</pre>



<pre class="code">
Code highlighting produced by Actipro CodeHighlighter (freeware)http://www.CodeHighlighter.com/-->using System;
using System.Data;
using System.Configuration;
using System.Web;
using System.Web.Security;
using System.Security.Cryptography;
using System.Web.UI;
using System.Web.UI.WebControls;
using System.Web.UI.WebControls.WebParts;
using System.Web.UI.HtmlControls;
using System.IO;
using System.Text;
/// <summary>
/// Summary description for DESEncrypt
/// </summary>
public class DESEncrypt
{
    private string iv = "12345678";
    private string key = "12345678";
    private Encoding encoding = new UnicodeEncoding();
    private DES des;

    public DESEncrypt()
    {
        des = new DESCryptoServiceProvider();
    }

    /// <summary>
    /// 设置加密密钥
    /// </summary>
    public string EncryptKey
    {
        get { return this.key; }
        set
        {
            this.key = value;
        }        
    }

    /// <summary>
    /// 要加密字符的编码模式
    /// </summary>
    public Encoding EncodingMode
    {
        get { return this.encoding; }
        set { this.encoding = value; }
    }

    /// <summary>
    /// 加密字符串并返回加密后的结果
    /// </summary>
    /// <param name="str"></param>
    /// <returns></returns>
    public string EncryptString(string str)
    {
        byte[] ivb = Encoding.ASCII.GetBytes(this.iv);
        byte[] keyb = Encoding.ASCII.GetBytes(this.EncryptKey);//得到加密密钥
        byte[] toEncrypt = this.EncodingMode.GetBytes(str);//得到要加密的内容
        byte[] encrypted;
        ICryptoTransform encryptor = des.CreateEncryptor(keyb, ivb);
        MemoryStream msEncrypt = new MemoryStream();
        CryptoStream csEncrypt = new CryptoStream(msEncrypt, encryptor, CryptoStreamMode.Write);
        csEncrypt.Write(toEncrypt, 0, toEncrypt.Length);
        csEncrypt.FlushFinalBlock();
        encrypted = msEncrypt.ToArray();
        csEncrypt.Close();
        msEncrypt.Close();
        return this.EncodingMode.GetString(encrypted);
    }
}
</pre>


<pre class="code">
Code highlighting produced by Actipro CodeHighlighter (freeware)http://www.CodeHighlighter.com/-->//根据用户名得到用户信息       
 DataTable dt = WYTWeb.UserDAO.UserLogin(userName);
 if (dt.Rows.Count == 0)
 {
    return -2;//用户不存在
 }

 DataRow row = dt.Rows[0];
 //得到密匙
 string salt = row["salt"].ToString();
 //验证密码是否正确
 if (EncryptPassWord.EncryptPwd(password, salt) == row["password"].ToString())
 {
      //登录成功
 }





Code highlighting produced by Actipro CodeHighlighter (freeware)http://www.CodeHighlighter.com/--> //从基类获得登录id
 int userId = LoginUser_Id;
 //获得密匙
 string salt = EncryptPassWord.CreateSalt();
 //得到经过加密后的"密码"
 string password = EncryptPassWord.EncryptPwd(txtPassword.Text.Trim(), salt);
 //修改原数据
 int result = WYTWeb.UserDAO.EditPassword(userId, password, salt);
 if (result > 0)
 {
     WYTWeb.LogDAO.InsertLog("info","wytWeb","用户"+userId+"修改了密码", userId ,this.Request.UserHostAddress.ToString());
     ShowMessage("密码修改成功");
           //this.Response.Redirect("CompanyInfo.aspx"); 
 }
 else
 {
    WYTWeb.LogDAO.InsertLog("info", "wytWeb", "用户" + userId + "修改密码失败", userId, this.Request.UserHostAddress.ToString());
    ShowMessage("密码修改失败");
 }

</pre>
