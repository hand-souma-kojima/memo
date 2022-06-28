
[【C#】指定したパスがファイルかフォルダ（ディレクトリ）かを確認する方法](https://baba-s.hatenablog.com/entry/2017/12/06/222859)<br>
[「プロジェクト」ウィンドウの現在選択されているフォルダを取得する方法](https://answers.unity.com/questions/472808/how-to-get-the-current-selected-folder-of-project.html)<br>
[【C#】ファイルパスからディレクトリを取得する](http://www.openreference.org/articles/view/444#:~:text=C%23%20%E3%81%A7%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%83%91%E3%82%B9%E3%81%8B%E3%82%89,%E5%91%BC%E3%81%B3%E5%87%BA%E3%81%99%E3%81%93%E3%81%A8%E3%81%8C%E3%81%A7%E3%81%8D%E3%81%BE%E3%81%99%E3%80%82)<br>
[【Unity】選択中のフォルダとすべてのサブフォルダのパスを取得するエディタ拡張](https://baba-s.hatenablog.com/entry/2020/09/29/080000)


~~~
private Object GetSelectFolder()
{
    // 選択したフォルダを取得
    var selectAssets = Selection.GetFiltered<DefaultAsset>(SelectionMode.DeepAssets);
    // 選択したアセットを取得
    var asset = Selection.activeObject;
    DefaultAsset folder = (selectAssets.Length != 0) ? selectAssets[0] : null;

    if (asset == null && folder == null) return m_createFolder;

    var obj = (asset != null) ? asset : folder;

    // パスを取得
    string path = AssetDatabase.GetAssetPath(obj.GetInstanceID());

    // パスからディレクトリを取得する（フォルダの場合はそのまま）
    if (path.Length > 0)
    {
        var isDirectory = File
            .GetAttributes(path)
            .HasFlag(FileAttributes.Directory);

        if (isDirectory)
        {
            return obj;
        }
        else
        {
            string dirName = System.IO.Path.GetDirectoryName(path);
            var directoryAsset = AssetDatabase.LoadAssetAtPath<UnityEngine.Object>(dirName);
            return directoryAsset;
        }
    }

    return m_createFolder;
}
~~~