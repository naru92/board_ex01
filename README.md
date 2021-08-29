# login Board 
## 로그인 게시판 2021-06-22 작성

** 특이사항 ** 
@requestParam
게시판 번호의 값, 게시물의 번호,  페이징시 페이지 값을 받기위해 사용



@ModelAttribute
CRUD구현시 특정 게시물의 객체를 이름으로 구분하기 위해 사용 
EX) 글쓰기 - writeContentBean
    글수정 - modifyContentBean
    
```
	@GetMapping("/write")
	public String write(@ModelAttribute("writeContentBean") ContentBean writeContentBean,
			@RequestParam("board_info_idx") int board_info_idx) {

		writeContentBean.setContent_board_idx(board_info_idx);

		return "board/write";
	}
    
    @GetMapping("/modify")
	public String modify(@RequestParam("board_info_idx") int board_info_idx,
			@RequestParam("content_idx") int content_idx,
			@ModelAttribute("modifyContentBean") ContentBean modifyContentBean, Model model) {

		model.addAttribute("board_info_idx", board_info_idx);
		model.addAttribute("content_idx", content_idx);

		ContentBean tempContentBean = boardService.getContentInfo(content_idx);

		modifyContentBean.setContent_writer_name(tempContentBean.getContent_writer_name());
		modifyContentBean.setContent_date(tempContentBean.getContent_date());
		modifyContentBean.setContent_subject(tempContentBean.getContent_subject());
		modifyContentBean.setContent_text(tempContentBean.getContent_text());
		modifyContentBean.setContent_file(tempContentBean.getContent_file());
		modifyContentBean.setContent_writer_idx(tempContentBean.getContent_writer_idx());
		modifyContentBean.setContent_board_idx(board_info_idx);
		modifyContentBean.setContent_idx(content_idx);

		return "board/modify";
	}
```

@Valid
유효성 검사시 JSR-303 제약 조건을 사용

```
//UserBean.java
@Size(min=2, max=4)
	@Pattern(regexp = "[가-힣]*")
	private String user_name;
	
	@Size(min=4, max=20)
	@Pattern(regexp = "[a-zA-Z0-9]*")
	private String user_id;
	
	@Size(min=4, max=20)
	@Pattern(regexp = "[a-zA-Z0-9]*")
	private String user_pw;
	
	@Size(min=4, max=20)
	@Pattern(regexp = "[a-zA-Z0-9]*")
	private String user_pw2;
	
	private boolean userIdExist;
	private boolean userLogin;
```

```
//ContentBean.java
@NotBlank
	private String content_subject;
	
	@NotBlank
	private String content_text;
	
	//브라우저가 보낸 파일데이터를 담을 변수
	private MultipartFile upload_file;
	
	private String content_file;
	private int content_writer_idx;
	private int content_board_idx;
	private String content_date;
	private String content_writer_name;

```



