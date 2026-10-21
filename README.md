<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>전화번호 안내</title>

<style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    background: #f4f6f9;
    font-family: Arial, "Noto Sans KR", sans-serif;
    color: #222;
}

.container {
    max-width: 1200px;
    margin: auto;
    padding: 30px 20px;
}

/* =========================
   상단 영역
========================= */

.header {
    background: white;
    border-radius: 18px;
    padding: 30px;
    box-shadow: 0 4px 15px rgba(0,0,0,0.08);
}

.header h1 {
    margin: 0;
    font-size: 30px;
}

.header p {
    margin: 8px 0 0;
    color: #777;
}

/* =========================
   검색창
========================= */

.search-area {
    display: flex;
    gap: 10px;
    margin-top: 25px;
}

.search-input {
    flex: 1;
    height: 55px;
    border: 2px solid #ddd;
    border-radius: 12px;
    padding: 0 18px;
    font-size: 17px;
    outline: none;
}

.search-input:focus {
    border-color: #3769d5;
}

.search-btn {
    width: 110px;
    border: 0;
    border-radius: 12px;
    background: #3769d5;
    color: white;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
}

.search-btn:hover {
    background: #2855b5;
}

/* =========================
   필터
========================= */

.filter-area {
    display: flex;
    gap: 10px;
    margin-top: 15px;
}

.filter-area select {
    height: 42px;
    border: 1px solid #ddd;
    border-radius: 8px;
    padding: 0 12px;
    background: white;
    min-width: 160px;
}

/* =========================
   결과
========================= */

.result-header {
    display: flex;
    justify-content: space-between;
    margin: 22px 5px 12px;
    color: #666;
}

.result-list {
    display: grid;
    grid-template-columns:
        repeat(auto-fill, minmax(320px, 1fr));
    gap: 15px;
}

/* =========================
   전화번호 카드
========================= */

.phone-card {
    background: white;
    border-radius: 15px;
    padding: 20px;

    box-shadow:
        0 2px 10px rgba(0,0,0,0.06);

    transition: 0.2s;
}

.phone-card:hover {
    transform: translateY(-2px);

    box-shadow:
        0 5px 18px rgba(0,0,0,0.10);
}

.department {
    color: #3769d5;
    font-size: 14px;
    font-weight: bold;
}

.name {
    font-size: 21px;
    font-weight: bold;
    margin-top: 7px;
    margin-bottom: 15px;
}

.info {
    display: grid;
    grid-template-columns: 65px 1fr;
    gap: 7px;

    font-size: 14px;
}

.label {
    color: #999;
}

.phone-area {
    display: flex;
    align-items: center;
    gap: 8px;

    margin-top: 18px;
}

.phone {
    flex: 1;

    color: #222;
    text-decoration: none;

    font-size: 21px;
    font-weight: bold;
}

.phone:hover {
    color: #3769d5;
}

.copy-btn {
    border: 0;
    background: #edf1f7;
    padding: 8px 12px;

    border-radius: 7px;

    cursor: pointer;
}

.copy-btn:hover {
    background: #dfe5ef;
}

/* =========================
   검색어 강조
========================= */

mark {
    background: #fff09c;
    padding: 1px 3px;
    border-radius: 3px;
}

/* =========================
   검색 결과 없음
========================= */

.no-result {
    grid-column: 1 / -1;

    background: white;

    border-radius: 15px;

    padding: 60px 20px;

    text-align: center;

    color: #777;
}

/* =========================
   모바일
========================= */

@media (max-width: 600px) {

    .container {
        padding: 15px;
    }

    .header {
        padding: 20px;
    }

    .header h1 {
        font-size: 25px;
    }

    .search-area {
        flex-direction: column;
    }

    .search-btn {
        width: 100%;
        height: 48px;
    }

    .filter-area {
        flex-direction: column;
    }

    .filter-area select {
        width: 100%;
    }

    .result-list {
        grid-template-columns: 1fr;
    }
}
</style>
</head>


<body>

<div class="container">

    <!-- =========================
         제목
    ========================== -->

    <div class="header">

        <h1>📞 전화번호 안내</h1>

        <p>
            이름 또는 전화번호를 입력하여 검색하세요.
        </p>


        <!-- =========================
             검색
        ========================== -->

        <div class="search-area">

            <input
                id="searchInput"
                class="search-input"
                type="text"
                placeholder="이름 또는 전화번호 검색"
            >

            <button
                class="search-btn"
                onclick="searchPhone()"
            >
                검색
            </button>

        </div>


        <!-- =========================
             부서 / 구분 필터
        ========================== -->

        <div class="filter-area">

            <select id="departmentFilter">
                <option value="">
                    전체 부서
                </option>
            </select>


            <select id="typeFilter">
                <option value="">
                    전체 구분
                </option>
            </select>

        </div>

    </div>


    <!-- =========================
         검색 결과 표시
    ========================== -->

    <div class="result-header">

        <span id="resultCount">
            검색 결과 0건
        </span>

        <span>
            전체
            <strong id="totalCount">0</strong>건
        </span>

    </div>


    <!-- 검색 결과 -->
    <div
        id="resultList"
        class="result-list"
    ></div>

</div>


<script>

/* ==================================================
   전화번호 데이터

   실제 엑셀 데이터를 아래 형식으로 입력
================================================== */

const DATA = [

    {
        department: "안전보건팀",
        name: "홍길동",
        type: "직원",
        phone: "033-000-0000",
        location: "본관 2층"
    },

    {
        department: "시설관리팀",
        name: "김철수",
        type: "직원",
        phone: "033-000-0001",
        location: "별관 1층"
    },

    {
        department: "간호국",
        name: "이영희",
        type: "직원",
        phone: "033-000-0002",
        location: "본관 3층"
    }

];


/* ==================================================
   HTML 특수문자 처리
================================================== */

function escapeHtml(text) {

    return String(text || "")
        .replace(/&/g, "&amp;")
        .replace(/</g, "&lt;")
        .replace(/>/g, "&gt;")
        .replace(/"/g, "&quot;")
        .replace(/'/g, "&#039;");

}


/* ==================================================
   검색어 정리
================================================== */

function normalize(text) {

    return String(text || "")
        .toLowerCase()
        .replace(/\s+/g, "");

}


/* ==================================================
   검색어 강조
================================================== */

function highlight(text, keyword) {

    let result =
        escapeHtml(text);

    if (!keyword) {
        return result;
    }

    const words =
        keyword
            .trim()
            .split(/\s+/)
            .filter(Boolean);

    words.forEach(word => {

        const safeWord =
            word.replace(
                /[.*+?^${}()|[\]\\]/g,
                "\\$&"
            );

        result =
            result.replace(
                new RegExp(
                    "(" + safeWord + ")",
                    "gi"
                ),
                "<mark>$1</mark>"
            );

    });

    return result;

}


/* ==================================================
   부서 목록 만들기
================================================== */

function makeDepartmentList() {

    const select =
        document.getElementById(
            "departmentFilter"
        );

    const departments =
        [...new Set(
            DATA
                .map(item => item.department)
                .filter(Boolean)
        )]
        .sort(
            (a,b) =>
            a.localeCompare(b, "ko")
        );


    departments.forEach(department => {

        const option =
            document.createElement("option");

        option.value =
            department;

        option.textContent =
            department;

        select.appendChild(option);

    });

}


/* ==================================================
   구분 목록 만들기
================================================== */

function makeTypeList() {

    const select =
        document.getElementById(
            "typeFilter"
        );

    const types =
        [...new Set(
            DATA
                .map(item => item.type)
                .filter(Boolean)
        )]
        .sort(
            (a,b) =>
            a.localeCompare(b, "ko")
        );


    types.forEach(type => {

        const option =
            document.createElement("option");

        option.value =
            type;

        option.textContent =
            type;

        select.appendChild(option);

    });

}


/* ==================================================
   전화번호 검색
================================================== */

function searchPhone() {

    const keyword =
        normalize(
            document.getElementById(
                "searchInput"
            ).value
        );


    const department =
        document.getElementById(
            "departmentFilter"
        ).value;


    const type =
        document.getElementById(
            "typeFilter"
        ).value;


    const result =
        DATA.filter(item => {

            const text =
                normalize(
                    [
                        item.department,
                        item.name,
                        item.type,
                        item.phone,
                        item.location
                    ].join(" ")
                );


            const keywordMatch =
                keyword === "" ||
                text.includes(keyword);


            const departmentMatch =
                department === "" ||
                item.department === department;


            const typeMatch =
                type === "" ||
                item.type === type;


            return (
                keywordMatch &&
                departmentMatch &&
                typeMatch
            );

        });


    displayResult(
        result,
        keyword
    );

}


/* ==================================================
   검색 결과 표시
================================================== */

function displayResult(
    data,
    keyword
) {

    const list =
        document.getElementById(
            "resultList"
        );


    const count =
        document.getElementById(
            "resultCount"
        );


    count.textContent =
        "검색 결과 " +
        data.length.toLocaleString() +
        "건";


    if (data.length === 0) {

        list.innerHTML = `

            <div class="no-result">

                검색 결과가 없습니다.

                <br><br>

                이름, 전화번호 또는
                부서명을 확인해주세요.

            </div>

        `;

        return;

    }


    list.innerHTML =
        data.map(item => {

            const phone =
                item.phone || "-";


            const tel =
                phone.replace(
                    /[^0-9+#*]/g,
                    ""
                );


            return `

                <div class="phone-card">

                    <div class="department">

                        ${highlight(
                            item.department,
                            keyword
                        )}

                    </div>


                    <div class="name">

                        ${highlight(
                            item.name,
                            keyword
                        )}

                    </div>


                    <div class="info">

                        <div class="label">
                            구분
                        </div>

                        <div>
                            ${highlight(
                                item.type,
                                keyword
                            )}
                        </div>


                        <div class="label">
                            위치
                        </div>

                        <div>
                            ${highlight(
                                item.location,
                                keyword
                            )}
                        </div>

                    </div>


                    <div class="phone-area">

                        <a
                            class="phone"
                            href="tel:${tel}"
                        >

                            ☎
                            ${highlight(
                                phone,
                                keyword
                            )}

                        </a>


                        <button
                            class="copy-btn"
                            onclick="copyPhone('${phone}')"
                        >
                            복사
                        </button>

                    </div>

                </div>

            `;

        }).join("");

}


/* ==================================================
   전화번호 복사
================================================== */

function copyPhone(phone) {

    navigator.clipboard
        .writeText(phone)
        .then(() => {

            alert(
                "전화번호가 복사되었습니다."
            );

        })
        .catch(() => {

            prompt(
                "전화번호를 복사하세요.",
                phone
            );

        });

}


/* ==================================================
   엔터키 검색
================================================== */

document
    .getElementById("searchInput")
    .addEventListener(
        "keydown",
        function(event) {

            if (event.key === "Enter") {

                searchPhone();

            }

        }
    );


/* ==================================================
   필터 변경 시 자동 검색
================================================== */

document
    .getElementById("departmentFilter")
    .addEventListener(
        "change",
        searchPhone
    );


document
    .getElementById("typeFilter")
    .addEventListener(
        "change",
        searchPhone
    );


/* ==================================================
   초기 실행
================================================== */

document.getElementById(
    "totalCount"
).textContent =
    DATA.length.toLocaleString();


makeDepartmentList();

makeTypeList();

searchPhone();

</script>

</body>
</html>
