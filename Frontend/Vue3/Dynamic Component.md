
一般情况下，如果切换component，component会销毁。包括数据和method， 可以用unmount hook去测试
如果不想销毁component， 可以用keeplive标签，这时可以用 activate和deactivate标签
