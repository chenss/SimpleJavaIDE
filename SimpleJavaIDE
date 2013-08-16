//Simple Java IDE based on JDK
//http://www.oschina.net/code/snippet_558212_15111
import java.awt.*;
import java.awt.event.*;
import java.io.*;

class MyMenuBar extends MenuBar {
	public MyMenuBar(Frame parent) {
		parent.setMenuBar(this);
	}

	public void addMenus(String[] menus) {
		for (int i = 0; i < menus.length; i++)
			add(new Menu(menus[i]));
	}

	public void addMenuItems(int menuNumber, String[] items) {
		for (int i = 0; i < items.length; i++) {
			if (items[i] != null)
				getMenu(menuNumber).add(new MenuItem(items[i]));
			else
				getMenu(menuNumber).addSeparator();
		}
	}

	public void addActionListener(ActionListener al) {
		for (int i = 0; i < getMenuCount(); i++)
			for (int j = 0; j < getMenu(i).getItemCount(); j++)
				getMenu(i).getItem(j).addActionListener(al);
		// 为菜单项注册动作时间监听器
	}
}

class MyFile {
	FileDialog fDlg;
	// FileDialog 类显示一个对话框窗口，用户可以从中选择文件。
	String s1;

	public MyFile(Frame parent, String s1) {
		fDlg = new FileDialog(parent, "", FileDialog.LOAD);
		// LOAD,此常量值指示文件对话框窗口的作用是查找要读取的文件。
		this.s1 = s1;
	}

	public String getPath() {
		return fDlg.getDirectory() + "\\" + fDlg.getFile();
	}

	public String getData() throws IOException
	// 当发生某种 I/O 异常时，抛出此异常。此类为异常的通用类，它是由失败的或中断的 I/O 操作生成的。
	{
		fDlg.setTitle("打开");
		fDlg.setMode(FileDialog.LOAD);
		// LOAD,此常量值指示文件对话框窗口的作用是查找要读取的文件。
		fDlg.setVisible(true);
		BufferedReader br = new BufferedReader(new FileReader(getPath()));
		// FileReader是用来读取字符文件的便捷类
		// BufferedReader是从字符输入流中读取文本，缓冲各个字符，从而提供字符、数组和行的高效读取。
		StringBuffer sb = new StringBuffer();
		// StringBuffer是线程安全的可变字符序列
		String aline;
		while ((aline = br.readLine()) != null)
			sb.append(aline + '\n');
		// 当存在换行时,输出换行符号:\n
		br.close();
		// 关闭字符输入流
		return sb.toString();
		// 返回字符序列
	}

	public void setData(String data) throws IOException
	// 当发生某种 I/O 异常时，抛出此异常。此类为异常的通用类，它是由失败的或中断的 I/O 操作生成的。
	{
		fDlg.setTitle("保存");
		fDlg.setMode(FileDialog.SAVE);
		// SAVE,此常量值指示文件对话框窗口的作用是查找要写入的文件。
		new File(s1);
		fDlg.setDirectory(s1);
		fDlg.setVisible(true);
		BufferedWriter bw = new BufferedWriter(new FileWriter(getPath()));
		// 将文本写入字符输出流，缓冲各个字符，从而提供单个字符、数组和字符串的高效写入。
		bw.write(data);
		// 写入字符串
		bw.close();
		// 关闭该流
	}
}

class MyFindDialog extends Dialog implements ActionListener {
	Label lFind = new Label("查找字符串");
	Label lReplace = new Label("替换字符串");
	TextField tFind = new TextField(10);
	TextField tReplace = new TextField(10);
	Button bFind = new Button("查找");
	Button bReplace = new Button("替换");
	TextArea ta;

	public MyFindDialog(Frame owner, TextArea ta) {
		super(owner, "查找", false);
		this.ta = ta;
		setLayout(null);
		lFind.setBounds(10, 30, 80, 20);
		lReplace.setBounds(10, 70, 80, 20);
		tFind.setBounds(90, 30, 90, 20);
		tReplace.setBounds(90, 70, 90, 20);
		bFind.setBounds(190, 30, 80, 20);
		bReplace.setBounds(190, 70, 80, 20);
		add(tFind);
		add(bFind);
		add(lReplace);
		add(tReplace);
		add(bReplace);
		setResizable(false);
		bFind.addActionListener(this);
		bReplace.addActionListener(this);
		tFind.addActionListener(this);
		tReplace.addActionListener(this);
		addWindowListener(new WindowAdapter() {
			public void windowClosing(WindowEvent e) {
				MyFindDialog.this.dispose();
			}
		});
	}// 构造函数结束

	public void showFind() {
		setTitle("查找");
		setSize(280, 60);
		setVisible(true);
	}

	public void showReplace() {
		setTitle("替换");
		setSize(280, 110);
		setVisible(true);
	}

	public void find() {
		String text = ta.getText();
		String str = tFind.getText();
		int end = text.length();
		int len = str.length();
		int start = ta.getSelectionEnd();
		if (start == end)
			start = 0;
		for (; start <= end - len; start++) {
			if (text.substring(start, start + len).equals(str)) {
				ta.setSelectionStart(start);
				ta.setSelectionEnd(start + len);
				return;
			}
		}
		// 若找不到待查字符串，则将光标置于末尾
		ta.setSelectionStart(end);
		ta.setSelectionEnd(end);
	}

	public Button getBFind() {
		return bFind;
	}

	public void replace() {
		String str = tReplace.getText();
		if (ta.getSelectedText().equals(tFind.getText()))
			ta.replaceRange(str, ta.getSelectionStart(), ta.getSelectionEnd());
		else
			find();
	}

	public void actionPerformed(ActionEvent e) {
		if (e.getSource() == bFind || e.getSource() == tFind) {
			find();
		} else if (e.getSource() == bReplace || e.getSource() == tReplace) {
			replace();
		}
	}
}

public class SimpleJavaIDE extends Frame implements ActionListener, KeyListener {
	String s = "/*Example*/\n" + "public class E\n" + "{\n"
			+ "\tpublic static void main(String args[])\n" + "\t{\n"
			+ "\t\tSystem.out.println(\"Hello,Java\");\n" + "\t}\n" + "}";
	// 设置默认的显示的代码
	static String s1 = "C:\\1000";
	// 设置默认路径为：C:\1000
	static boolean boo = true;
	static FileDialog fDlg;
	// FileDialog 类显示一个对话框窗口，用户可以从中选择文件。
	TextArea editor = new TextArea(s);
	// 可编辑的editor
	TextArea editor1 = new TextArea();
	MyFile mf = new MyFile(this, s1);
	// MyFile对象
	MyFindDialog findDlg = new MyFindDialog(this, editor);

	public SimpleJavaIDE(String title) {
		super(title);
		MyMenuBar mb = new MyMenuBar(this);
		// 添加需要的菜单及菜单项
		mb.addMenus(new String[] { "文件", "编辑", "查找", "编译" });
		mb.addMenuItems(0, new String[] { "新建", null, "打开" });
		mb.addMenuItems(1, new String[] { "清除", null, "全选", null, "清空" });
		mb.addMenuItems(2, new String[] { "查找", null, "替换" });
		mb.addMenuItems(3, new String[] { "保存", null, "调试", "运行" });
		editor.setFont(new Font("Calibri", Font.PLAIN, 20));
		editor1.setFont(new Font("Arial", Font.PLAIN, 12));
		setBounds(200, 150, 750, 470);
		add(editor, BorderLayout.CENTER);
		add(editor1, BorderLayout.SOUTH);
		editor.addKeyListener(this);
		// 文本区editor注册监视器
		editor1.setEditable(false);
		// 设置editor1不可编辑
		setVisible(true);
		validate();
		mb.addActionListener(this);
		// 为菜单项注册动作时间监听器
		addWindowListener(new WindowAdapter() {
			public void windowClosing(WindowEvent e) {
				System.exit(0);
			}
		});
	}

	public void actionPerformed(ActionEvent e) {
		String selected = e.getActionCommand();
		// 获取菜单项标题
		if (selected.equals("新建"))
			editor.setText("");
		else if (selected.equals("打开")) {
			try {
				editor.setText(mf.getData());
			} catch (IOException ie) {
				editor1.append("\n系统出错！");
			}
		} else if (selected.equals("保存")) {
			try {
				mf.setData(editor.getText());
			} catch (IOException ie) {
				editor1.append("\n系统出错！");
			}
		} else if (selected.equals("调试")) {
			try {
				Runtime ce = Runtime.getRuntime();
				Process process = ce.exec("javac " + mf.getPath());

				BufferedInputStream is = new BufferedInputStream(
						process.getErrorStream());
				byte[] buf = new byte[4096];
				int n = -1;
				boolean boo1 = true;
				while ((n = is.read(buf, 0, buf.length)) != -1) {
					editor1.append('\n' + new String(buf, 0, n));
					// 将错误流输出
					boo = false;
					boo1 = false;
				}
				if (boo1) {
					editor1.append("\n调试成功！");
					boo = true;
				} else {
					editor1.append("\n调试失败！");
					boo1 = true;
				}
				is.close();
				process.destroy();
			} catch (Exception e1) {
				editor1.append("\n系统出错！");
			}
		} else if (selected.equals("运行")) {
			if (boo) {
				try {
					Runtime ce = Runtime.getRuntime();
					byte[] buf = new byte[4096];
					int n = -1;
					String s1 = mf.getPath();
					int i=s1.lastIndexOf("\\");
					int j=s1.lastIndexOf(".");
					String s2 = s1.substring(i+1, j);
					//取出文件名，存入s2
					Process process1 = ce.exec("java " + s2);
					BufferedInputStream bis = new BufferedInputStream(
							process1.getInputStream());
					while ((n = bis.read(buf, 0, buf.length)) != -1) {
						editor1.append('\n' + new String(buf, 0, n));
					}
					bis.close();
					process1.destroy();
					//杀死进程
				} catch (Exception e2) {
					editor1.append("\n系统出错！");
				}
			} else
				editor1.append("\n请先调试！");
		}  else if (selected.equals("清除")) {
			editor.replaceRange("", editor.getSelectionStart(),
					editor.getSelectionEnd());
		} else if (selected.equals("清空")) {
			editor1.setText(null);
		} else if (selected.equals("全选")) {
			editor.setSelectionStart(0);
			editor.setSelectionEnd(editor.getText().length());
		} else if (selected.equals("查找")) {
			findDlg.showFind();
		} else if (selected.equals("替换")) {
			findDlg.showReplace();
		}
	}

	public void keyPressed(KeyEvent e3) {
		if (e3.getModifiers() == InputEvent.CTRL_MASK
				&& e3.getKeyCode() == KeyEvent.VK_S) {
			try {
				mf.setData(editor.getText());
			} catch (IOException ie) {
				editor1.append("\n系统出错！");
			}
		}
	}

	public void keyTyped(KeyEvent e4) {
	}

	public void keyReleased(KeyEvent e5) {
	}

	public static void main(String[] args) {
		new SimpleJavaIDE("简单的Java IDE");
	}
}
