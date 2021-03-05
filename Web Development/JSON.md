# JSON(JavaScript Object Notation)
경량의 DATA-교환 형식. 사람이 읽고 쓰기에 용이하며, 기계가 분석하고 생성하기도 용이하다.

# JSON 구조
두개의 구조를 기본으로 가진다.

* name/value 형태의 쌍으로 collection 타입
    * 다양한 언어들에서, object, struct, dictionary 등으로 실현되어있다
* 값들의 순서화된 리스트. 
    * 대부분의 언어에서 array, vector, list 등으로 실현되어있다

# 예시
    {
        "squadName": "Super hero squad",
        "homeTown": "Metro City",
        "formed": 2016,
        "secretBase": "Super tower",
        "active": true,
        "members": [
            {
            "name": "Molecule Man",
            "age": 29,
            "secretIdentity": "Dan Jukes",
            "powers": [
                "Radiation resistance",
                "Turning tiny",
                "Radiation blast"
            ]
            },
            {
            "name": "Madame Uppercut",
            "age": 39,
            "secretIdentity": "Jane Wilson",
            "powers": [
                "Million tonne punch",
                "Damage resistance",
                "Superhuman reflexes"
            ]
            },
            {
            "name": "Eternal Flame",
            "age": 1000000,
            "secretIdentity": "Unknown",
            "powers": [
                "Immortality",
                "Heat Immunity",
                "Inferno",
                "Teleportation",
                "Interdimensional travel"
            ]
            }
        ]
    }