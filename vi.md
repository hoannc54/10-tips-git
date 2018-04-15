#10 mẹo để bạn có thể nâng cao kĩ năng Git lên cấp độ tiếp theo

Gần đây, chúng tôi đã xuất bản một vài hướng dẫn để giúp bạn làm quen với nền tảng Git và sử dụng Git trong môi trường làm việc nhóm. 
Các lệnh mà chúng ta thảo luận là đủ để giúp một nhà phát triển tồn tại trong thế giới Git. Trong bài đăng này, chúng tôi sẽ cố gắng khám phá cách quản lý thời gian một cách hiệu quả và tận dụng các tính năng mà Git cung cấp.

Lưu ý: Một số lệnh trong bài viết này bao gồm một phần của lệnh trong dấu ngoặc vuông (e.g. git add -p [file_name]). Trong những ví dụ này, bạn sẽ chèn số cần thiết, định danh, vv.. mà không có dấu ngoặc vuông.

##1. Git Auto Completion

Nếu bạn chạy lệnh Git thông qua command line, đó là một nhiệm vụ mệt mỏi để gõ các lệnh bằng tay mỗi lần. Để giúp đỡ việc này, bạn có thể kích hoạt tự động hoàn thành các lệnh Git trong vòng vài phút.

Để có được kịch bản, hãy chạy lệnh sau trong hệ thống Unix:

```
cd ~
curl https://raw.github.com/git/git/master/contrib/completion/git-completion.bash -o ~/.git-completion.bash
```

Tiếp theo, thêm các dòng sau vào```~/.bash_profile``` file:

```
if [ -f ~/.git-completion.bash ]; then
    . ~/.git-completion.bash
fi
```

Mặc dù tôi đã đề cập đến điều này trước đó, Tôi không thể đủ động lực: Nếu bạn muốn sử dụng các tính năng của Git đầy đủ, bạn chắc chắn nên chuyển sang giao diện dòng lệnh!

##2. Loại bỏ các file trong Git 

Bạn có mệt mỏi với các tập tin biên dịch (giống như ```.pyc```) xuất hiện trong kho Git của bạn? Hoặc bạn có chán đến mức bạn đã thêm chúng vào Git? Nhìn xa hơn, có một cách thông qua đó bạn có thể nói với Git để bỏ qua các tập tin nhất định và thư mục chung. Đơn giản chỉ cần tạo một tập tin có tên ```.gitignore``` và liệt kê các tệp tin và thư mục mà bạn không muốn Git theo dõi. Bạn có thể thực hiện ngoại lệ bằng cách sử dụng dấu chấm than (!).

```
*.pyc
*.exe
my_db_config/

!main.pyc
```

##3. Who Messed With My Code?

Đó là bản năng tự nhiên của con người để đổ lỗi cho người khác khi có điều gì đó không ổn. Nếu máy chủ sản xuất của bạn bị hỏng, rất dễ để tìm ra thủ phạm - chỉ cần thực hiện ```git blame```.Câu lệnh này sẽ cho bạn thấy tác giả của mỗi dòng trong 1 file, commit thực hiện thay đổi cuối cùng của dòng đó, và mốc thời gian của commit. 

```
git blame [file_name]

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946443git-ninja-01.png)

Và trong ảnh chụp màn hình bên dưới, bạn có thể thấy lệnh này sẽ trông như thế nào trên một kho lưu trữ lớn hơn:

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946441git-ninja-02.png)

##4. Duyệt lại lịch sử của Repository

Chúng tôi đã xem xét việc sử dụng ```git log``` trong một hướng dẫn trước đây, tuy nhiên, có ba lựa chọn mà bạn nên biết.

* ```--oneline``` – Nén thông tin hiển thị bên cạnh mỗi cam kết với một cam kết giảm bớt và thông báo cam kết, tất cả được hiển thị trong một dòng đơn.
* ```--graph``` – Tùy chọn này rút ra một biểu diễn đồ họa dựa trên văn bản của lịch sử ở phía bên tay trái của đầu ra. Không sử dụng nếu bạn đang xem lịch sử cho một nhánh.
* ```--all``` – Cho thấy lịch sử của tất cả các chi nhánh.

Dưới đây là những gì kết hợp các tùy chọn như sau:

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946444git-ninja-03.png)

##5. Không bao giờ mất theo dõi của một Commit

Giả sử bạn đã commit một cái gì mà bạn không muốn và kết thúc bằng việc thực hiện hard reset để quay trở lại trạng thái trước. 
Sau đó, bạn nhận thấy bạn đã mất một số thông tin khác trong tiến trình và muốn khôi phục nó, hay ít nhất là xem nó. Đây là lúc ```git reflog``` có thể giúp bạn.

```git log``` đơn giản cho bạn biết commit mới nhất, trước đó, trước đó nữa, và vân vân. Tuy nhiên, ```git reflog``` là 1 danh sách các commit mà đã trỏ tới ở đầu.
 Hãy nhớ rằng nó nằm ở dưới hệ thống của bạn, nó không phải là 1 phần của repository của bạn và không bao gồm các push hay merge.

Nếu tôi chạy ```git log```, tôi sẽ lấy được các commit là 1 phần của repository của tôi

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946446git-ninja-04.png)

Tuy nhiên, 1 ```git reflog``` hiển thị 1 commit (```b1b0ee9 - HEAD@{4}```) mà bị mất khi bạn thực hiện 1 hard reset.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946447git-ninja-05.png)

##6. Phân loại các phần của một tệp đã thay đổi cho một Commit

Nói chung là một thực hành tốt để làm cho feature-based commits,có nghĩa là mỗi commit phải đại diện cho một tính năng hoặc sửa lỗi. 
Hãy xem xét điều gì sẽ xảy ra nếu bạn đã sửa hai lỗi, hoặc thêm nhiều tính năng mà không commit những thay đổi. Trong hoàn cảnh tình hình như vậy, 
bạn có thể đặt những thay đổi trong một commit duy nhất.Nhưng có một cách tốt hơn: Xếp các tập tin riêng ra và commit chúng một cách riêng biệt.

Giả sử bạn đã thực hiện nhiều thay đổi cho một tệp và muốn chúng xuất hiện trong các commit riêng biệt. Trong trường hợp đó, chúng ta thêm các tập tin bằng tiền tố  ```-p``` để thêm trong lệnh của chúng ta.

```

git add -p [file_name]

```

Chúng ta hãy cùng xem mô tả tương tự. Tôi đã thêm ba dòng mới vào ```file_name``` và tôi chỉ muốn những dòng đầu tiên và thứ ba xuất hiện trong commit của tôi.Chúng ta hãy cùng xem ```git diff``` cho chúng ta thấy.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946449git-ninja-06.png)

Và hãy xem điều gì xảy ra khi chúng ta thềm tiền tố ```-p``` cho câu lệnh ```add```.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946450git-ninja-07.png)

Dường như Git cho rằng tất cả những thay đổi đều là một phần của cùng một ý tưởng, do đó cho nó vào một nhóm lớn. Bạn có những lựa chọn sau đây:

* Nhập y để stage nhóm đó
* Nhập n để không stage nhóm đó
* Nhập e để tự sửa nhóm đó
* Nhập d để thoát hoặc đi tới tệp tiếp theo.
* Nhập s để chia nhóm đó.

Trong trường hợp của chúng tôi, chúng tôi chắc chắn muốn chia nhỏ nó thành các phần nhỏ hơn để bổ sung có chọn lọc và bỏ qua phần còn lại.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946452git-ninja-08.png)

Bạn có thể thấy, chúng tôi đã thêm vào dòng đầu tiên và thứ ba và bỏ qua thứ hai. Sau đó bạn có thể xem tình trạng của repository và thực hiện một commit.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946454git-ninja-09.png)

##7. Squash nhiều commit

Khi bạn gửi đi code của bạn để xem lại và tạo một pull request( thử xảy ra thường xuyên trong các dự án open source), bạn có thể sẽ được yêu cầu để tạo 1 thay đổi cho code của bạn trước khi nó được chấp nhận. Bạn tạo sự thay đổi, chỉ khi được yêu cầu thay đổi nó 1 lần nữa trong lần xem lại tiếp theo. Trước khi bạn biết nó, bạn có vài commit thêm. Lý tưởng nhất là bạn có thể ép chúng lại làm 1 sử dụng câu lệnh ```rebase```.

```

git rebase -i HEAD~[number_of_commits]

```

Nếu bạn muốn squash hai lần cuối commit, bạn chạy là lệnh như sau.

```

git rebase -i HEAD~2

```

Khi chạy lệnh này, bạn được đưa đến một giao diện tương tác liệt kê các commit hỏi bạn cái nào squash. Lý tưởng nhất là bạn ```pick``` các commit mới nhất và ```squash``` cái cũ.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946455git-ninja-10.png)

Sau đó bạn được yêu cầu cung cấp thông báo commit cho commit mới. Quá trình này chủ yếu viết lại lịch sử commit của bạn.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946457git-ninja-11.png)

##8. Stash Uncommitted Changes

Hãy nói rằng bạn đang làm việc với một lỗi nhất định và một tính năng, và bạn đột nhiên được yêu cầu mô tả công việc của bạn. Công việc hiện tại chưa đủ hoàn thành commit, 
và bạn không thể đưa ra một mô tả ở giai đoạn này (mà không cần trở lại các thay đổi). Trong trường hợp như vậy, ```git stash``` nó sẽ giải cứu bạn. Stash cơ bản thực hiện tất cả các thay đổi của bạn và lưu giữ chúng để sử dụng tiếp.
 Để giấu các thay đổi của bạn, bạn chỉ cần chạy-

```

git stash

```

Để kiểm tra danh sách các stashes, bạn có thể chạy như sau:

```

git stash list

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946458git-ninja-12.png)

Nếu bạn muốn bỏ cất và khôi phục nhưng thay đổi những thay đổi chưa commit, bạn gọi stash:

```

git stash apply

```

Trong ảnh chụp màn hình mới nhất, bạn có thể thấy rằng mỗi stash có một định danh, một số duy nhất (mặc dù chúng ta chỉ có một phần trong trường hợp này).
 Trong trường hợp bạn muốn áp dụng chỉ stashes chọn lọc, bạn thêm định danh cụ thể vào câu lệnh ```apply```:

```

git stash apply stash@{2}

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946461git-ninja-13.png)


##9. Kiểm tra các commit đã mất

Mặc dù ```reflog``` là một cách để kiểm tra các commit bị mất, nó không khả thi trong các repository lớn. Đó là khi ```fsck``` (file system check) lệnh này vào cuộc.

```

git fsck --lost-found

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946463git-ninja-14.png)

Ở đây bạn có thể thấy một commit bị mất. Bạn có thể kiểm tra các thay đổi trong commit bằng cách chạy ```git show [commit_hash]``` hoặc khôi phục nó bằng cách chạy ```git merge [commit_hash]```.

```git fsck``` có ích hơn ```reflog```. Giả sử bạn đã xóa một remote branch and then cloned the repository. Với ```fsck``` bạn có thể tìm kiếm và phục hồi các bị xóa remote branch.


##10. Cherry Pick

Tôi đã lưu lại các câu lệnh Git thanh lịch nhất cuối cùng. Các câu lệnh ```cherry-pick``` là do lệnh Git yêu thích của tôi, vì ý nghĩa nghĩa đen cũng như tiện ích của nó!

Với những giới hạn đơn giản nhất, ```cherry-pick``` sẽ chọn 1 commit đơn lẻ từ các nhánh khác nhau và hợp chúng với cái hiện tại. Nếu bạn đang làm việc theo cách song sóng trên 2 hay nhiều hơn nhánh, bạn có thể chú ý 1 lỗi mà xuất hiện ở tất cả các nhánh. Nếu bạn giải quyết nó trong 1, bạn có thể cherry pick commit đến cácnhánh khác, mà không làm rỗi loạn với các file hay commit khác

Hãy hình dung 1 kịch bản khi bạn có thể gọi nó. Tôi có 2 nhánh và tôi muốn```cherry-pick``` các commit ```b20fd14: Cleaned junk``` vào một cái khác.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946465git-ninja-15.png)

Tôi chuyển sang chi nhánh mà tôi muốn ```cherry-pick``` các commit, và chạy như sau:

```

git cherry-pick [commit_hash]

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946467git-ninja-16.png)

Mặc dù chúng tôi đã có một ```cherry-pick``` vào thời gian này, bạn nên biết rằng lệnh này thường có thể dẫn đến xung đột, do đó, sử dụng nó cẩn thận.

##Kết luận

Với cái này,chúng tôi đến cuối danh sách các mẹo của chúng tôi mà tôi nghĩ rằng có thể giúp bạn lấy kỹ năng Git của bạn đến một cấp độ mới. 
Git là tốt nhất hiện có và nó có thể hoàn thành bất cứ điều gì bạn có thể tưởng tượng. Vì vậy, luôn cố gắng để thử thách chính mình với Git. Cơ hội đến, bạn sẽ có thể học được điều gì đó mới mẻ!

