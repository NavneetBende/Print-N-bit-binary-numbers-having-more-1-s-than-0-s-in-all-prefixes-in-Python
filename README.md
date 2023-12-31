# Print-N-bit-binary-numbers-having-more-1-s-than-0-s-in-all-prefixes-in-Python

N-bit binary numbers having more  or equal 1’s than 0’s in Python
Here, on this page, we will discuss the program to print N-bit binary numbers having more or equal 1’s than 0’s in Python programming language. We are given an integer value n and need to print n-bit binary numbers.

Example :

Input : 4
Output : 1111 1110 1101 1100 1011 1010
Explanation : 1111 1110 1101 1100 1011 1010 all these are the required binary numbers
N-bit binary numbers having more or equal 1’s than 0’s

Method Discussed :
Method 1 : Using Recursive Way
Method 2 : Using  Non-Recursive Way
Method 1:
In this method we use recursion. At each point in the recursion, we append 0 and 1 to the partially formed number and recur with one less digit. 

N-bit binary numbers having more or equal 1’s than 0’s in Python
Python Code
Run
def printRec(number, extraOnes, remainingPlaces):
    if 0 == remainingPlaces:
        print(number, end=" ")
        return

    printRec(number + "1", extraOnes + 1, remainingPlaces - 1)

    if 0 < extraOnes:
        printRec(number + "0", extraOnes - 1, remainingPlaces - 1)


def printNums(n):
    str = ""
    printRec(str, 0, n)


n = 4
printNums(n)
Output
1111 1110 1101 1100 1011 1010 
Method 2:
In this non-recursive method, the idea is to directly generate the numbers in the range of 2^N to 2^(N-1), then require only these which satisfy the condition

Run
def getBinaryRep(N, num_of_bits):
    r = ""
    num_of_bits -= 1

    while num_of_bits >= 0:
        if N & (1 << num_of_bits):
            r += "1"
        else:
            r += "0"
        num_of_bits -= 1

    return r


def NBitBinary(N):
    r = []
    first = 1 << (N - 1)
    last = first * 2

    for i in range(last - 1, first - 1, -1):
        zero_cnt = 0
        one_cnt = 0
        t = i
        num_of_bits = 0

        while t:
            if t & 1:
                one_cnt += 1
            else:
                zero_cnt += 1
            num_of_bits += 1
            t = t >> 1

        if one_cnt >= zero_cnt:

            all_prefix_match = True
            msk = (1 << num_of_bits) - 2
            prefix_shift = 1

            while msk:
                prefix = ((msk & i) >> prefix_shift)
                prefix_one_cnt = 0
                prefix_zero_cnt = 0

                while prefix:
                    if prefix & 1:
                        prefix_one_cnt += 1
                    else:
                        prefix_zero_cnt += 1
                    prefix = prefix >> 1

                if prefix_zero_cnt > prefix_one_cnt:
                    all_prefix_match = False
                    break

                prefix_shift += 1
                msk = msk & (msk << 1)

            if all_prefix_match:
                r.append(getBinaryRep(i, num_of_bits))
    return r


n = 4
results = NBitBinary(n)
for i in range(len(results)):
    print(results[i], end=" ")
Output
1111 1110 1101 1100 1011 1010 
